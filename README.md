import glob, os
import numpy as np
import pandas as pd
from sklearn.linear_model import LogisticRegression
from sklearn.preprocessing import OneHotEncoder
from sklearn.metrics import roc_auc_score, log_loss

FILE = None                                  # None = detectar solo; o pon el nombre exacto
CORTES = ["2025-06-30", "2025-08-31"]        # fecha_fin maxima que entra al modelo
TEST_MESES = 2                               # los ultimos N meses antes del corte son el test
MIN_N = 100
MIN_N_COL = {"estado": 300}                  # estados chicos dan coeficientes de puro ruido

# valor_residual y pago_final fuera: "sin_dato" solo delata el producto (Credit no los tiene)
NUM = ["plazo", "precio_auto", "monto_financiar", "tasa", "mensualidad", "enganche", "pct_enganche"]
CAT = ["tipo_persona", "contratos_previos", "comportamiento_pago", "estado",
       "empresa", "marca", "auto", "unidad", "producto"]
NUMB = NUM + ["edad_al_inicio", "ltv"]
CLASES = {0: "abandono", 1: "refinanciar", 2: "renovar"}

# ---------- 0. Localizar el archivo ----------
def find_file():
    cands = []
    for pat in ["*.xlsx", "/mnt/data/*.xlsx", "/mnt/user-data/**/*.xlsx", "**/*.xlsx"]:
        cands += glob.glob(pat, recursive=True)
    for f in dict.fromkeys(cands):
        nm = os.path.basename(f).lower()
        if nm.startswith(("scorecard", "analisis", "aclaraciones", "resultado", "comparacion", "~$")):
            continue
        try:
            cols = set(pd.read_excel(f, sheet_name="data", nrows=0).columns)
        except Exception:
            continue
        if {"renovo", "refinancio", "fecha_fin"} <= cols:
            return f
    raise FileNotFoundError("No encontre un .xlsx con hoja 'data'. Pon el nombre en FILE.")

FILE = FILE or find_file()
print("Archivo usado:", FILE)

# ---------- 1. Carga y objetivo (vacio = no lo hizo) ----------
df = pd.read_excel(FILE, sheet_name="data")
for c in ["fecha_nacimiento", "fecha_inicio", "fecha_fin", "fecha_renovacion"]:
    df[c] = pd.to_datetime(df[c], errors="coerce")

def si(col):
    return df[col].astype(str).str.strip().str.lower().isin(["si", "sí"])

# Renovar manda sobre refinanciar cuando ocurren ambos
df["y"] = np.where(si("renovo"), 2, np.where(si("refinancio"), 1, 0))

# ---------- 2. Features ----------
def base(x):
    X = pd.DataFrame(index=x.index)
    for c in NUM:
        X[c] = pd.to_numeric(x[c], errors="coerce")
    # Edad al firmar, no a hoy: el valor no cambia con el tiempo
    X["edad_al_inicio"] = (x["fecha_inicio"] - x["fecha_nacimiento"]).dt.days / 365.25
    X["ltv"] = X["monto_financiar"] / X["precio_auto"]
    for c in CAT:
        X[c] = x[c].astype(str).str.strip()
    # SIN PB / Sin PB / VACIO son la misma categoria escrita distinto
    X["comportamiento_pago"] = X["comportamiento_pago"].str.upper().replace(
        {"SIN PB": "SIN_DATO", "VACÍO": "SIN_DATO", "VACIO": "SIN_DATO",
         "NAN": "SIN_DATO", "NONE": "SIN_DATO", "": "SIN_DATO"})
    return X

def run(corte):
    corte = pd.Timestamp(corte)
    d = df[df["fecha_fin"].notna() & (df["fecha_fin"] <= corte)].copy()
    ini_test = corte - pd.DateOffset(months=TEST_MESES)
    tr = (d["fecha_fin"] <= ini_test).to_numpy()
    X = base(d)
    Xtr, Xte = X[tr], X[~tr]
    y = d["y"].to_numpy()
    ytr, yte = y[tr], y[~tr]

    # Reglas de agrupacion aprendidas SOLO con train
    rules = {}
    for c in NUMB:
        s = Xtr[c].dropna()
        rules[c] = ("raw", None) if s.nunique() <= 12 else \
                   ("bins", np.unique(s.quantile([.25, .5, .75]).values))
    for c in CAT:
        vc = Xtr[c].value_counts()
        rules[c] = ("cat", set(vc[vc >= MIN_N_COL.get(c, MIN_N)].index))

    def transform(Xi):
        Z = pd.DataFrame(index=Xi.index)
        for c, (kind, v) in rules.items():
            if kind == "raw":
                Z[c] = Xi[c].map(lambda t: "sin_dato" if pd.isna(t) else str(t))
            elif kind == "bins":
                Z[c] = pd.cut(Xi[c], [-np.inf, *v, np.inf]).astype(str).replace("nan", "sin_dato")
            else:
                Z[c] = Xi[c].where(Xi[c].isin(v), "otros")
        return Z

    Ztr, Zte = transform(Xtr), transform(Xte)
    try:
        enc = OneHotEncoder(handle_unknown="ignore", sparse_output=False)
    except TypeError:
        enc = OneHotEncoder(handle_unknown="ignore", sparse=False)
    A, Ate = enc.fit_transform(Ztr), enc.transform(Zte)

    m = LogisticRegression(C=0.5, max_iter=3000).fit(A, ytr)
    assert list(m.classes_) == [0, 1, 2]
    Ptr, Pte = m.predict_proba(A), m.predict_proba(Ate)

    auc = {k: roc_auc_score(yte == k, Pte[:, k]) for k in CLASES}
    prior = np.bincount(ytr, minlength=3) / len(ytr)
    ll_m = log_loss(yte, Pte, labels=[0, 1, 2])
    ll_b = log_loss(yte, np.tile(prior, (len(yte), 1)), labels=[0, 1, 2])

    # Refinanciar solo donde se puede (Finance + Leasing)
    prod_te = d.loc[~tr, "producto"].astype(str).str.strip().to_numpy()
    mk = np.isin(prod_te, ["Finance", "Leasing"])
    n_ref = int((yte[mk] == 1).sum())
    auc_ref_fl = roc_auc_score(yte[mk] == 1, Pte[mk, 1]) if n_ref >= 30 and (yte[mk] == 1).mean() < 1 else np.nan

    # Grado de churn = terciles de prob_abandono (train). Es RELATIVO, no absoluto
    cg = np.quantile(Ptr[:, 0], [1 / 3, 2 / 3])
    grado = np.where(Pte[:, 0] <= cg[0], "Bajo", np.where(Pte[:, 0] <= cg[1], "Medio", "Alto"))
    cal_grado = pd.DataFrame({"grado_churn": grado, "abandono": yte == 0,
        "refinanciar": yte == 1, "renovar": yte == 2}).groupby("grado_churn").agg(
        n=("abandono", "size"), tasa_abandono=("abandono", "mean"),
        tasa_refinanciar=("refinanciar", "mean"), tasa_renovar=("renovar", "mean")).round(4).reset_index()

    # Tabla de coeficientes (auditable)
    rows, i = [], 0
    for col, cats in zip(Ztr.columns, enc.categories_):
        for cat in cats:
            sub = (Ztr[col] == cat).to_numpy()
            n = int(sub.sum())
            rows.append([col, cat, n,
                         *[round(float(m.coef_[k, i]), 6) for k in CLASES],
                         *[round(float((ytr[sub] == k).mean()), 4) if n else np.nan for k in CLASES]])
            i += 1
    coef = pd.DataFrame(rows, columns=["variable", "valor", "n_train",
        "coef_abandono", "coef_refinanciar", "coef_renovar",
        "tasa_abandono_train", "tasa_refinanciar_train", "tasa_renovar_train"])
    interc = pd.DataFrame({"clase": list(CLASES.values()), "intercepto": m.intercept_.round(6)})
    cortes = pd.DataFrame([(c, ";".join(map(str, v))) for c, (k, v) in rules.items() if k == "bins"],
                          columns=["variable", "cortes"])

    mes = d.assign(mes=d["fecha_fin"].dt.to_period("M").astype(str)).groupby("mes").agg(
        contratos=("y", "size"), renovar=("y", lambda s: (s == 2).mean()),
        refinanciar=("y", lambda s: (s == 1).mean()),
        abandono=("y", lambda s: (s == 0).mean())).round(4).reset_index()

    res = pd.DataFrame({"metrica": [
        "corte_fecha_fin", "inicio_test", "filas_total", "train", "test",
        "tasa_train_abandono", "tasa_train_refinanciar", "tasa_train_renovar",
        "tasa_test_abandono", "tasa_test_refinanciar", "tasa_test_renovar",
        "AUC_abandono", "AUC_renovar", "AUC_refinanciar_global",
        "AUC_refinanciar_solo_Finance_Leasing", "refinanciados_test_FL",
        "logloss_modelo", "logloss_sin_modelo", "corte_grado_1", "corte_grado_2"],
        "valor": [str(corte.date()), str(ini_test.date()), len(d), int(tr.sum()), int((~tr).sum()),
                  *[round(float((ytr == k).mean()), 4) for k in CLASES],
                  *[round(float((yte == k).mean()), 4) for k in CLASES],
                  round(auc[0], 4), round(auc[2], 4), round(auc[1], 4),
                  round(auc_ref_fl, 4) if not np.isnan(auc_ref_fl) else "n/d", n_ref,
                  round(ll_m, 4), round(ll_b, 4), round(float(cg[0]), 4), round(float(cg[1]), 4)]})

    out = f"scorecard_v4_{corte.date()}.xlsx"
    with pd.ExcelWriter(out) as w:
        for nm, t in [("resumen", res), ("tasa_por_mes", mes), ("calibracion_grado", cal_grado),
                      ("tabla_coeficientes", coef), ("interceptos", interc), ("cortes_numericos", cortes)]:
            t.to_excel(w, sheet_name=nm, index=False)
    return res, cal_grado, mes

# ---------- 3. Correr cada corte y comparar ----------
comp = None
for c in CORTES:
    res, cal, mes = run(c)
    print("\n===== CORTE", c, "=====")
    print(res.to_string(index=False))
    print("\nCALIBRACION POR GRADO\n", cal.to_string(index=False))
    print("\nTASA POR MES\n", mes.to_string(index=False))
    r = res.set_index("metrica")["valor"].rename(c)
    comp = r.to_frame() if comp is None else comp.join(r)
comp.reset_index().to_excel("comparacion_cortes.xlsx", index=False)
print("\nCOMPARACION DE CORTES\n", comp.to_string())
