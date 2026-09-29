Quiero preparar este documento Word para utilizarlo posteriormente como plantilla en Power Automate mediante la acción "Populate a Microsoft Word template".

IMPORTANTE:
No quiero que rediseñes, reestructures, resumas ni recrees el documento.

Debes conservar exactamente:
- Portada.
- Logotipos.
- Colores.
- Tipografías.
- Tamaños.
- Tablas.
- Bordes.
- Encabezados.
- Pies de página.
- Saltos de página.
- Secciones.
- Textos corporativos.
- Orden del contenido.
- Distribución visual actual.

El objetivo es únicamente convertir los espacios que posteriormente recibirán información dinámica en campos identificables para Power Automate.

Utiliza controles de contenido de texto sin formato (Plain Text Content Controls) siempre que sea posible.

Configura los siguientes campos:

DATOS GENERALES
- Nombre de la iniciativa → NombreIniciativa
- Área solicitante → AreaSolicitante
- Solicitante → Solicitante
- Sponsor → Sponsor
- Fecha → Fecha

OBJETIVO / PARA QUÉ
- Objetivo de negocio → ObjetivoNegocio
- Resultado esperado → ResultadoEsperado

PROBLEMA ACTUAL
- Situación actual → SituacionActual
- Principal dolor o necesidad → DolorPrincipal
- Consecuencias relevantes → Consecuencias

PROCESO ACTUAL - AS-IS
- Flujo actual → FlujoASIS
- Actores → ActoresASIS
- Herramientas → HerramientasASIS
- Principales puntos de fricción → FriccionesASIS

PROCESO FUTURO - TO-BE
- Flujo futuro esperado → FlujoTOBE
- Participación humana → ParticipacionHumana
- Participación automatizada → ParticipacionAutomatizada
- Resultado futuro → ResultadoFuturo

ALCANCE
- VW Servicios Sí → VWServiciosSi
- VW Servicios No → VWServiciosNo
- VW Leasing Sí → VWLeasingSi
- VW Leasing No → VWLeasingNo
- VW Bank Sí → VWBankSi
- VW Bank No → VWBankNo
- VW IB Sí → VWIBSi
- VW IB No → VWIBNo
- Productos y perfiles de clientes impactados → ProductosPerfiles
- Marcas impactadas → Marcas
- Usuarios y áreas involucradas → UsuariosAreas

IMPACTO
- Principales beneficios esperados → Beneficios

Para la tabla de KPIs conservar exactamente las columnas:
KPI | Impacto esperado (+/- %) | Justificación | Área

Si es técnicamente posible, preparar la fila de datos mediante Repeating Section Content Control para permitir múltiples KPIs.

Dentro de la fila utilizar:
- KPI → KPI
- Impacto esperado → ImpactoKPI
- Justificación → JustificacionKPI
- Área → AreaKPI

SISTEMAS, DATOS Y DEPENDENCIAS
- Sistemas / herramientas involucrados → Sistemas
- Fuentes de información o datos → FuentesDatos
- Dependencias relevantes → Dependencias
- Integraciones o accesos requeridos → Integraciones

EVALUACIÓN INICIAL

Potencial de automatización:
- Alto → AutomatizacionAlto
- Medio → AutomatizacionMedio
- Bajo → AutomatizacionBajo
- Requiere análisis → AutomatizacionAnalisis

Posible uso de IA:
- Sí → IASi
- No → IANo
- Requiere análisis → IAAnalisis

Posible uso de agente:
- Sí → AgenteSi
- No → AgenteNo
- Requiere análisis → AgenteAnalisis

Alternativa sin IA:
- Sí → SinIASi
- No → SinIANo
- Requiere análisis → SinIAAnalisis

PRECLASIFICACIÓN
- Carril 1 - Self-Service → Carril1Marca
- Carril 2 - Champion Assisted → Carril2Marca
- Carril 3 - IT Assisted / Integration → Carril3Marca
- Justificación → JustificacionCarril

PENDIENTES Y SIGUIENTE PASO
- Pendientes por validar → Pendientes
- Siguiente paso recomendado → SiguientePaso
- Carril mostrado en el resumen final → Carril

REGLAS TÉCNICAS:

1. Cada Content Control debe tener un título único exactamente igual al nombre especificado.
2. No utilizar espacios, acentos ni caracteres especiales en los nombres de los Content Controls.
3. Para campos que pueden contener textos extensos, habilitar múltiples párrafos/retornos de carro cuando corresponda.
4. No utilizar Checkbox Content Controls para campos que serán llenados por Power Automate.
5. Para opciones Sí/No, evaluación y carriles utilizar Plain Text Content Controls que posteriormente puedan recibir "X" o quedar vacíos.
6. No escribir valores ficticios dentro de los campos.
7. No eliminar textos fijos de la plantilla.
8. No modificar el contenido corporativo.
9. No agregar nuevas secciones.
10. No cambiar la estructura visual.
11. No generar todavía información de ninguna iniciativa.
12. No completar los campos con ejemplos.

El resultado debe seguir viéndose prácticamente idéntico al documento original. La única modificación debe ser la incorporación de los Content Controls necesarios para que posteriormente Power Automate pueda rellenarlo.

Antes de realizar cualquier cambio que implique alterar la estructura visual del documento, conserva la estructura original y prioriza la compatibilidad con Power Automate.