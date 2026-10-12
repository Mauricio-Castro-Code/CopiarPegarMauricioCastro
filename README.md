import pandas as pd
from sklearn.ensemble import HistGradientBoostingClassifier
from sklearn.metrics import roc_auc_score, average_precision_score

hist = xl("tblHist[#All]", headers=True)

CIERRE_DIAS = 60  # un "No" antes de fecha_fin + esta ventana todavia puede cambiar a "Si"
HOY = pd.Timestamp.today().normalize()

CAT_COLS = ["tipo_persona", "contratos_previos", "cliente_tipo", "comportamiento_pago",
            "estado", "marca", "unidad", "producto"]
HIGH_CARD = ["empresa", "auto"]
NUM_COLS = ["plazo", "precio_auto", "monto_financiar", "tasa", "mensualidad",
            "pago_final", "valor_residual", "enganche", "pct_enganche"]
DATE_COLS = ["fecha_nacimiento", "fecha_inicio", "fecha_fin"]

def clean(df):
    df = df.copy()
    for c in DATE_COLS:
        df[c] = pd.to_datetime(df[c], errors="coerce")
    return df

def build_features(df, top):
    X = pd.DataFrame(index=df.index)
    for c in NUM_COLS:
        X[c] = pd.to_numeric(df[c], errors="coerce")
    # Edad al firmar, no a hoy: asi el valor no cambia con el tiempo
    X["edad_al_inicio"] = (df["fecha_inicio"] - df["fecha_nacimiento"]).dt.days / 365.25
    X["ltv"] = X["monto_financiar"] / X["precio_auto"]
    X["mes_fin"] = df["fecha_fin"].dt.month
    for c in CAT_COLS:
        X[c] = df[c].astype(str).str.strip()
    for c in HIGH_CARD:
        s = df[c].astype(str).str.strip()
        X[c] = s.where(s.isin(top[c]), "otros")
    return X

d = clean(hist)
txt = d["renovo"].astype(str).str.strip().str.lower()
vacio = d["renovo"].isna() | txt.isin(["", "nan", "none"])
renovo = txt.isin(["si", "sí"])
dijo_no = txt.isin(["no"])

# Entrena solo con desenlace explicito: Si, o No ya vencido. Los vacios quedan fuera.
vencido = d["fecha_fin"] + pd.Timedelta(days=CIERRE_DIAS) <= HOY
resuelto = renovo | (dijo_no & vencido)
n_vacios = int(vacio.sum())
n_no_sin_vencer = int((dijo_no & ~vencido).sum())
d, y = d[resuelto], renovo[resuelto].astype(int)

TOP = {c: d[c].astype(str).str.strip().value_counts().head(50).index for c in HIGH_CARD}
X = build_features(d, TOP)
CATS = {c: pd.CategoricalDtype(sorted(X[c].unique())) for c in CAT_COLS + HIGH_CARD}

def encode(X):
    X = X.copy()
    for c, t in CATS.items():
        X[c] = X[c].astype(t)  # categoria desconocida -> NaN, el modelo la tolera
    return X

X = encode(X)

# Validacion por tiempo: 75% mas antiguo entrena, 25% mas reciente prueba
corte = d["fecha_fin"].quantile(0.75)
tr = d["fecha_fin"] <= corte

def make_model():
    return HistGradientBoostingClassifier(
        categorical_features="from_dtype", learning_rate=0.05,
        max_iter=300, early_stopping=True, random_state=42)

m = make_model().fit(X[tr], y[tr])
p = m.predict_proba(X[~tr])[:, 1]

# Modelo final con todo el historico resuelto
model = make_model().fit(X, y)

pd.DataFrame({
    "metrica": ["AUC", "PR-AUC", "tasa_renovacion_prueba", "filas_entrenamiento", "filas_prueba",
                "excluidas_renovo_vacio", "excluidas_no_sin_vencer"],
    "valor": [round(roc_auc_score(y[~tr], p), 3), round(average_precision_score(y[~tr], p), 3),
              round(y[~tr].mean(), 3), int(tr.sum()), int((~tr).sum()),
              n_vacios, n_no_sin_vencer]
})
