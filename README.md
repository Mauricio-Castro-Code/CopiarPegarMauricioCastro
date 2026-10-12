Antes de seguir, necesito aclarar cuatro puntos:

1. ¿Por qué la fecha observable es 2025-12-30? Dime cuál es la fecha máxima de fecha_fin, de fecha_renovacion y de fecha_inicio en la base, y cuántos contratos hay por trimestre de fecha_fin con su tasa de renovación.
2. La tasa de renovación pasa de 13.5% (train) a 23.0% (test). Explícame por qué: tasa por trimestre de fecha_fin, y por producto, marca y contratos_previos dentro de train y test. Indica si cambió la mezcla de clientes o la tasa dentro de cada segmento.
3. El score 0-100 tiene AUC 0.682 y la probabilidad logística 0.650. ¿Qué variables o rangos difieren entre los dos? ¿Se ajustó algo mirando el test? Si es así, rehazlo sin tocar el test.
4. Muéstrame la tabla de puntos (variable | valor | puntos | n_contratos | tasa histórica) y la calibración por banda de score en el test (n contratos y tasa real). Define los grados Bajo/Medio/Alto con terciles del score, no con umbrales fijos.

Muéstrame el código que ejecutaste.
