Contexto: base de contratos de financiamiento automotriz (leasing, crédito, financiamiento). Cada fila es un contrato; el objetivo es saber si el cliente renovó. Hoja "data".

Diccionario de columnas:
- cliente: ID del cliente (identificador, no usar en el modelo)
- tipo_persona: PFA, PFP, PM, etc. (persona física con actividad, sin actividad, persona moral)
- contratos_previos: 1, 2 o 3+
- cliente_tipo: nuevo o recurrente
- comportamiento_pago: A, B, E (categoría, no numérica; A = mejor)
- estado: estado de la República
- fecha_nacimiento, contrato (ID), empresa, marca (VW, SEAT, AUDI), auto, unidad (nuevo/usado)
- producto: leasing, crédito, financiamiento
- fecha_inicio, fecha_fin, plazo (meses), precio_auto, monto_financiar, tasa, mensualidad, pago_final, valor_residual, enganche, pct_enganche
- refinancio (sí/no): NO usar, puede ocurrir junto con la renovación
- renovo (sí/no): OBJETIVO
- contrato_renovado, fecha_renovacion, marca_renovacion, producto_renovacion: solo existen si renovó. NO usar (fuga de datos)
- churn, grado_churn: columnas vacías de salida, ignorarlas

Parte 1, análisis descriptivo:
- Excluye filas con renovo vacío (no las cuentes como "No").
- Calcula la tasa de renovación por: tipo_persona, cliente_tipo, contratos_previos, comportamiento_pago, marca, producto, unidad, estado, plazo (en rangos) y pct_enganche (en rangos).
- Muestra siempre el número de contratos de cada grupo y omite grupos con menos de 100.
- Marca los 10 grupos que más se alejan de la tasa global de renovación, tanto hacia arriba como hacia abajo.
- Revisa combinaciones de dos variables (ej. tipo_persona × producto, marca × unidad) y reporta las más llamativas con su tamaño.
- Reporta los 15 modelos de auto (columna auto) con mayor y menor renovación, solo con 100+ contratos.

Parte 2, modelo:
- Gradient boosting con las columnas válidas. De fecha_nacimiento saca la edad al inicio; de fecha_fin saca el mes.
- Valida por tiempo: 75% más antiguo según fecha_fin entrena, 25% más reciente prueba.
- Reporta AUC, PR-AUC, tasa de renovación en prueba y las 10 variables más importantes.

Al final: muéstrame el código Python que ejecutaste y avísame si notas columnas sospechosas (que parezcan fuga de datos) o datos raros.
