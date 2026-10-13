import numpy as np
import pandas as pd
from sklearn.linear_model import LogisticRegression
from sklearn.preprocessing import OneHotEncoder
from sklearn.metrics import roc_auc_score

FILE = "data.xlsx"    # ajusta al nombre real del archivo adjunto
SHEET = "data"
MIN_N = 100           # categorias con menos contratos en train -> "otros"
OUT = "scorecard_v2.xlsx"

NUM = ["plazo", "precio_auto", "monto_financiar", "tasa", "mensualidad",
       "pago_final", "valor_residual", "enganche", "pct_enganche"]
# cliente_tipo se omite a proposito: es copia de contratos_previos (Cramer V = 1)
CAT = ["tipo_persona", "contratos_previos", "comportamiento_pago", "estado",
       "empresa", "marca", "auto", "unidad", "producto"]
NUMB = NUM + ["edad_al_inicio", "ltv"]

# ---------- 1. Carga y objetivo ----------
df = pd.read_excel(FILE, sheet_name=SHEET)
for c in ["fecha_nacimiento", "fecha_inicio", "fecha_fin", "fecha_renovacion"]:
    df[c] = pd.to_datetime(df[c], errors="coerce")

txt = df["renovo"].astype(str).str.strip().str.lower()
df["y"] = np.select([txt.isin(["si", "sí"]), txt.eq("no")], [1.0, 0.0], default=np.nan)

# ---------- 2. Lag entre fecha_fin y renovacion ----------
ren = df[df["y"] == 1]
lag = (ren["fecha_renovacion"] - ren["fecha_fin"]).dt.days
lag_stats = lag.describe(percentiles=[.5, .9, .99]).to_frame("lag_dias")
lag_stats.loc["pct_lag_mayor_0"] = (lag > 0).mean()
p99 = lag.quantile(.99)
CIERRE = 0 if pd.isna(p99) else int(max(0, np.ceil(p99)))  # dias de espera tras fecha_fin
fecha_obs = df["fecha_renovacion"].max()

# ---------- 3. Tasa por trimestre (todas las filas) ----------
df["trim"] = df["fecha_fin"].dt.to_period("Q").astype(str)
trim = df.groupby("trim").agg(
    contratos=("y", "size"),
    renovo_vacio=("y", lambda s: int(s.isna().sum())),
    tasa_renov=("y", "mean"),
).reset_index()

# ---------- 4. Universo de entrenamiento ----------
# Si: siempre. No: solo si ya paso la ventana de cierre. Vacios: fuera.
vencido = df["fecha_fin"] + pd.Timedelta(days=CIERRE) <= fecha_obs
resuelto = (df["y"] == 1) | ((df["y"] == 0) & vencido)
d = df[resuelto].copy()
y = d["y"].astype(int)
n_vacios = int(df["y"].isna().sum())
n_no_pend = int(((df["y"] == 0) & ~vencido).sum())

def base(x):
    X = pd.DataFrame(index=x.index)
    for c in NUM:
        X[c] = pd.to_numeric(x[c], errors="coerce")
    X["edad_al_inicio"] = (x["fecha_inicio"] - x["fecha_nacimiento"]).dt.days / 365.25
    X["ltv"] = X["monto_financiar"] / X["precio_auto"]
    X["mes_fin"] = x["fecha_fin"].dt.month.astype("Int64").astype(str)
    for c in CAT:
        X[c] = x[c].astype(str).str.strip()
    return X

# ---------- 5. Split por tiempo (75% antiguo / 25% reciente) ----------
corte = d["fecha_fin"].quantile(0.75)
tr = d["fecha_fin"] <= corte
Xtr, Xte = base(d[tr]), base(d[~tr])

# Reglas de agrupacion aprendidas SOLO con train
rules = {}
for c in NUMB:
    s = Xtr[c].dropna()
    rules[c] = ("raw", None) if s.nunique() <= 12 else \
               ("bins", np.unique(s.quantile([.25, .5, .75]).values))
for c in CAT + ["mes_fin"]:
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

# ---------- 6. Modelo ----------
try:
    enc = OneHotEncoder(handle_unknown="ignore", sparse_output=False)
except TypeError:
    enc = OneHotEncoder(handle_unknown="ignore", sparse=False)
A = enc.fit_transform(Ztr)
m = LogisticRegression(C=0.5, max_iter=2000).fit(A, y[tr])

p_te = m.predict_proba(enc.transform(Zte))[:, 1]
auc_prob = roc_auc_score(y[~tr], p_te)

# Score 0-100 = percentil del puntaje bruto en train (transformacion monotona -> mismo AUC)
tr_raw = np.sort(m.decision_function(A) * 10)
def to_score(raw):
    return 100 * np.searchsorted(tr_raw, raw, side="right") / len(tr_raw)

s_te = to_score(m.decision_function(enc.transform(Zte)) * 10)
auc_score = roc_auc_score(y[~tr], s_te)

# score ALTO = mas retencion; churn es lo inverso
def grado_churn(s):
    return np.where(s <= 100 / 3, "Alto", np.where(s <= 200 / 3, "Medio", "Bajo"))

cal = pd.DataFrame({"score": s_te, "y": y[~tr].values})
cal["grado_churn"] = grado_churn(cal["score"].values)
cal_grado = cal.groupby("grado_churn").agg(n=("y", "size"), tasa_real_renov=("y", "mean")).reset_index()
cal["banda"] = pd.cut(cal["score"], [-0.01, 10, 20, 30, 40, 50, 60, 70, 80, 90, 100])
cal_banda = cal.groupby("banda", observed=True).agg(n=("y", "size"), tasa_real_renov=("y", "mean")).reset_index()
cal_banda["banda"] = cal_banda["banda"].astype(str)

# ---------- 7. Tabla de puntos (auditable) ----------
rows, i = [], 0
for col, cats in zip(Ztr.columns, enc.categories_):
    for cat in cats:
        sub = Ztr[col] == cat
        rows.append((col, cat, round(m.coef_[0][i] * 10, 2), int(sub.sum()),
                     round(y[tr][sub].mean(), 4)))
        i += 1
puntos = pd.DataFrame(rows, columns=["variable", "valor", "puntos", "n_train", "tasa_renov_train"])
puntos = puntos.sort_values(["variable", "puntos"], ascending=[True, False])

intercepto = round(float(m.intercept_[0]) * 10, 2)
pct_tab = pd.DataFrame({"percentil": range(1, 100),
                        "puntaje_bruto_minimo": np.percentile(tr_raw, range(1, 100)).round(2)})

resumen = pd.DataFrame({"metrica": [
    "fecha_observable", "cierre_dias_usado", "filas_total", "filas_entrenables",
    "excluidas_renovo_vacio", "excluidas_no_sin_vencer", "train", "test",
    "tasa_train", "tasa_test", "AUC_probabilidad", "AUC_score", "intercepto_puntos",
    "corte_train_hasta"],
    "valor": [str(fecha_obs.date()), CIERRE, len(df), len(d), n_vacios, n_no_pend,
              int(tr.sum()), int((~tr).sum()), round(y[tr].mean(), 4),
              round(y[~tr].mean(), 4), round(auc_prob, 4), round(auc_score, 4),
              intercepto, str(corte.date())]})

with pd.ExcelWriter(OUT) as w:
    resumen.to_excel(w, "resumen", index=False)
    lag_stats.reset_index().to_excel(w, "lag_renovacion", index=False)
    trim.to_excel(w, "tasa_por_trimestre", index=False)
    puntos.to_excel(w, "tabla_puntos", index=False)
    pct_tab.to_excel(w, "score_percentiles", index=False)
    cal_grado.to_excel(w, "calibracion_grado", index=False)
    cal_banda.to_excel(w, "calibracion_banda", index=False)

for name, t in [("RESUMEN", resumen), ("LAG", lag_stats.reset_index()),
                ("TRIMESTRES", trim), ("CALIBRACION GRADO", cal_grado),
                ("CALIBRACION BANDA", cal_banda)]:
    print("\n" + name)
    print(t.to_string(index=False))
