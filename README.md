Usa la hoja "data". Objetivo: construir una tabla de scoring de retención auditable.

Datos:
- Excluye filas con renovo vacío.
- Excluye del modelo: cliente, contrato, refinancio, contrato_renovado, fecha_renovacion, marca_renovacion, producto_renovacion, churn, grado_churn.
- Excluye los "No" cuyo fecha_fin + 60 días aún no ha pasado (pueden estar pendientes).
- fecha_nacimiento → edad al inicio. plazo, pct_enganche, monto_financiar y tasa → rangos por cuartiles.

Pasos:
1. Ajusta una regresión logística con las variables válidas. Si dos variables miden lo mismo (ej. cliente_tipo y contratos_previos), revisa su relación y quédate con la más informativa.
2. Valida por tiempo: el 75% más antiguo según fecha_fin entrena, el 25% más reciente prueba. Reporta AUC.
3. Convierte los coeficientes a puntos (score 0-100). Entrégame la tabla: variable | valor | puntos | n_contratos | tasa_renovacion_historica. Valores con menos de 100 contratos = 0 puntos.
4. Con el 25% de prueba, tabla de calibración por banda de score (0-20, 20-40, ... 80-100): n contratos y tasa real de renovación.
5. Muéstrame el código que ejecutaste.
