import numpy as np
import pandas as pd
from sklearn.linear_model import LogisticRegression
from sklearn.preprocessing import OneHotEncoder
from sklearn.metrics import roc_auc_score, log_loss

FILE, SHEET, OUT = "data.xlsx", "data", "scorecard_v3.xlsx"  # ajusta el nombre del archivo
MIN_N = 100  # categorias con menos contratos en train -> "otros"
NUM = ["plazo", "precio_auto", "monto_financiar", "tasa", "mensualidad",
       "pago_final", "valor_residual", "enganche", "pct_enganche"]
# cliente_tipo fuera (copia de contratos_previos); refinancio y las columnas *_renovacion fuera (objetivo/fuga)
CAT = ["tipo_persona", "contratos_previos", "comportamiento_pago", "estado",
       "empresa", "marca", "auto", "unidad", "producto"]
NUMB = NUM + ["edad_al_inicio", "ltv"]
CLASES = {0: "abandono", 1: "refinanciar", 2: "renovar"}

# ---------- 1. Carga y objetivo (vacio = no lo hizo) ----------
df = pd.read_excel(FILE, sheet_name=SHEET)
for c in ["fecha_nacimiento", "fecha_inicio", "fecha_fin", "fecha_renovacion"]:
    df[c] = pd.to_datetime(df[c], errors="coerce")

def si(col):
    return df[col].astype(str).str.strip().str.lower().isin(["si", "sí"])

# Renovar manda sobre refinanciar cuando ocurren ambos (250 casos)
df["y"] = np.where(si("renovo"), 2, np.where(si("refinancio"), 1, 0))

# ---------- 2. Chequeos de la regla "vacio = No" ----------
ren = si("renovo")
lag = (df.loc[ren, "fecha_renovacion"] - df.loc[ren, "fecha_fin"]).dt.days
chk = pd.DataFrame({"chequeo": [
    "no_renovo_con_fecha_renovacion (debe ser 0)",
    "no_renovo_con_contrato_renovado (debe ser 0)",
    "renovo_sin_fecha_renovacion (debe ser 0)",
    "renovo_y_refinancio_ambos",
    "lag_min_dias", "lag_p50_dias", "lag_p99_dias", "pct_lag_mayor_0"],
    "valor": [int(df.loc[~ren, "fecha_renovacion"].notna().sum()),
              int(df.loc[~ren, "contrato_renovado"].notna().sum()),
              int(df.loc[ren, "fecha_renovacion"].isna().sum()),
              int((ren & si("refinancio")).sum()),
              float(lag.min()), float(lag.quantile(.5)), float(lag.quantile(.99)),
              round(float((lag > 0).mean()), 4)]})

d = df[df["fecha_fin"].notna()].copy()
d["mes"] = d["fecha_fin"].dt.to_period("M").astype(str)
por_mes = d.groupby("mes").agg(
    contratos=("y", "size"),
    renovar=("y", lambda s: (s == 2).mean()),
    refinanciar=("y", lambda s: (s == 1).mean()),
    abandono=("y", lambda s: (s == 0).mean())).round(4).reset_index()

# ---------- 3. Features ----------
def base(x):
    X = pd.DataFrame(index=x.index)
    for c in NUM:
        X[c] = pd.to_numeric(x[c], errors="coerce")
    # Edad al firmar, no a hoy: el valor no cambia con el tiempo
    X["edad_al_inicio"] = (x["fecha_inicio"] - x["fecha_nacimiento"]).dt.days / 365.25
    X["ltv"] = X["monto_financiar"] / X["precio_auto"]
    for c in CAT:
        X[c] = x[c].astype(str).str.strip()
    return X

# ---------- 4. Split por tiempo (75% antiguo / 25% reciente) ----------
corte = d["fecha_fin"].quantile(0.75)
tr = d["fecha_fin"] <= corte
Xtr, Xte = base(d[tr]), base(d[~tr])
ytr, yte = d.loc[tr, "y"].to_numpy(), d.loc[~tr, "y"].to_numpy()

# Reglas de agrupacion aprendidas SOLO con train
rules = {}
for c in NUMB:
    s = Xtr[c].dropna()
    rules[c] = ("raw", None) if s.nunique() <= 12 else \
               ("bins", np.unique(s.quantile([.25, .5, .75]).values))
for c in CAT:
    vc = Xtr[c].value_counts()
    rules[c] = ("cat", set(vc[vc >= MIN_N].index))

def transform(X):
    Z = pd.DataFrame(index=X.index)
    for c, (kind, v) in rules.items():
        if kind == "raw":
            Z[c] = X[c].map(lambda t: "sin_dato" if pd.isna(t) else str(t))
        elif kind == "bins":
            Z[c] = pd.cut(X[c], [-np.inf, *v, np.inf]).astype(str).replace("nan", "sin_dato")
        else:
            Z[c] = X[c].where(X[c].isin(v), "otros")
    return Z

Ztr, Zte = transform(Xtr), transform(Xte)
try:
    enc = OneHotEncoder(handle_unknown="ignore", sparse_output=False)
except TypeError:
    enc = OneHotEncoder(handle_unknown="ignore", sparse=False)
A, Ate = enc.fit_transform(Ztr), enc.transform(Zte)

# ---------- 5. Modelo multinomial ----------
m = LogisticRegression(C=0.5, max_iter=3000).fit(A, ytr)
assert list(m.classes_) == [0, 1, 2]
Ptr, Pte = m.predict_proba(A), m.predict_proba(Ate)

auc = {k: roc_auc_score(yte == k, Pte[:, k]) for k in CLASES}
ll_model = log_loss(yte, Pte, labels=[0, 1, 2])
prior = np.bincount(ytr, minlength=3) / len(ytr)
ll_base = log_loss(yte, np.tile(prior, (len(yte), 1)), labels=[0, 1, 2])  # "sin modelo"

# Calibracion por decil, por clase
cal = []
for k, nombre in CLASES.items():
    t = pd.DataFrame({"p": Pte[:, k], "real": (yte == k).astype(int),
                      "decil": pd.qcut(Pte[:, k], 10, duplicates="drop")})
    g = t.groupby("decil", observed=True).agg(
        n=("real", "size"), prob_media=("p", "mean"), tasa_real=("real", "mean")).round(4).reset_index()
    g["decil"] = g["decil"].astype(str)
    g.insert(0, "clase", nombre)
    cal.append(g)
cal_clase = pd.concat(cal)

# Grado de churn = terciles de prob_abandono calculados en train
cg = np.quantile(Ptr[:, 0], [1 / 3, 2 / 3])
def grado(p):
    return np.where(p <= cg[0], "Bajo", np.where(p <= cg[1], "Medio", "Alto"))

cal_grado = pd.DataFrame({
    "grado_churn": grado(Pte[:, 0]), "abandono": yte == 0,
    "refinanciar": yte == 1, "renovar": yte == 2}).groupby("grado_churn").agg(
    n=("abandono", "size"), tasa_abandono=("abandono", "mean"),
    tasa_refinanciar=("refinanciar", "mean"), tasa_renovar=("renovar", "mean")).round(4).reset_index()

# ---------- 6. Tabla de coeficientes (auditable) ----------
rows, i = [], 0
for col, cats in zip(Ztr.columns, enc.categories_):
    for cat in cats:
        sub = (Ztr[col] == cat).to_numpy()
        n = int(sub.sum())
        rows.append([col, cat, n,
                     *[round(float(m.coef_[k, i]), 6) for k in CLASES],
                     *[round(float((ytr[sub] == k).mean()), 4) if n else np.nan for k in CLASES]])
        i += 1
puntos = pd.DataFrame(rows, columns=["variable", "valor", "n_train",
    "coef_abandono", "coef_refinanciar", "coef_renovar",
    "tasa_abandono_train", "tasa_refinanciar_train", "tasa_renovar_train"])
interceptos = pd.DataFrame({"clase": list(CLASES.values()),
                            "intercepto": [round(float(v), 6) for v in m.intercept_]})
cortes = pd.DataFrame([(c, ";".join(map(str, v))) for c, (k, v) in rules.items() if k == "bins"],
                      columns=["variable", "cortes"])

resumen = pd.DataFrame({"metrica": [
    "filas_total", "train", "test", "corte_train_hasta",
    "tasa_train_abandono", "tasa_train_refinanciar", "tasa_train_renovar",
    "tasa_test_abandono", "tasa_test_refinanciar", "tasa_test_renovar",
    "AUC_abandono", "AUC_refinanciar", "AUC_renovar",
    "logloss_modelo", "logloss_sin_modelo", "corte_grado_1", "corte_grado_2"],
    "valor": [len(d), int(tr.sum()), int((~tr).sum()), str(corte.date()),
              *[round(float((ytr == k).mean()), 4) for k in CLASES],
              *[round(float((yte == k).mean()), 4) for k in CLASES],
              *[round(auc[k], 4) for k in CLASES],
              round(ll_model, 4), round(ll_base, 4), round(float(cg[0]), 4), round(float(cg[1]), 4)]})

with pd.ExcelWriter(OUT) as w:
    for nombre, t in [("resumen", resumen), ("chequeos", chk), ("tasa_por_mes", por_mes),
                      ("tabla_coeficientes", puntos), ("interceptos", interceptos),
                      ("cortes_numericos", cortes), ("calibracion_clase", cal_clase),
                      ("calibracion_grado", cal_grado)]:
        t.to_excel(w, sheet_name=nombre, index=False)

for nombre, t in [("RESUMEN", resumen), ("CHEQUEOS", chk), ("TASA POR MES", por_mes),
                  ("CALIBRACION GRADO", cal_grado)]:
    print("\n" + nombre)
    print(t.to_string(index=False))
