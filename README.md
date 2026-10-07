# generate_vwfs_initiative_document

## Propósito
Generar el Formato de Iniciativa oficial de VWFS como documento Word completamente poblado con la información confirmada durante el discovery. Esta skill existe para evitar dos fallos: entregar un documento genérico/simplificado o devolver la plantilla oficial vacía.

## Cuándo usar esta skill
Úsala únicamente después de que el usuario confirme explícitamente el resumen final del discovery con “Confirmo” o una aceptación inequívoca. La confirmación autoriza automáticamente la generación; no pidas una segunda autorización ni preguntes si desea el Word.

## Fuentes obligatorias
- `2_VWFS_AI_Opportunity_Discovery_Template.docx`: estructura y diseño oficial.
- `4_Initiative_Template_Guidelines.docx`: reglas de mapeo y llenado.
- `1_Classification_Guidelines.docx`: carril, justificación y siguiente paso.
- Toda la conversación de discovery confirmada; no uses únicamente el último mensaje ni el resumen final.

## Secuencia
1. Verifica que exista confirmación explícita del resumen.
2. Antes del documento, muestra en chat el checklist de las 11 secciones: `✅ completo`, `⚠️ parcial`, `⬜ vacío`, `➖ no aplica`.
3. Usa el resultado de `classify_vwfs_initiative` para carril, motivo, dependencia principal, pendientes y siguiente paso.
4. Abre/usa la plantilla oficial como base. No crees un documento libre desde cero si la plantilla está disponible.
5. Recorre toda la conversación confirmada y construye un mapa de datos → campos de plantilla.
6. Sustituye cada placeholder para el que exista información confirmada.
7. Mantén cualquier campo no confirmado vacío. Usa “No aplica” solo si el usuario lo indicó explícitamente.
8. Registra los faltantes importantes como pendientes específicos y accionables.
9. Marca `[X]` en un solo carril.
10. Genera el Word final respetando diseño y estructura.
11. Ejecuta la verificación final antes de entregar.

## Mapeo obligatorio de las 11 secciones
### Portada
- Nombre de la iniciativa.
- Área solicitante.
- Solicitante/contacto.
- Sponsor.
- Líder de Negocio.
- Fecha de discovery.

### 1. Resumen ejecutivo
5–8 líneas con problema, objetivo, solución futura esperada, impacto y carril preliminar.

### 2. Objetivo / Para qué
Propósito y resultado de negocio. No describas aquí arquitectura o tecnología.

### 3. Problema actual
Situación actual, dolor/ineficiencia, consecuencias confirmadas y riesgo de no actuar. No inventes consecuencias.

### 4. Proceso actual — AS-IS
Flujo actual, actores, entradas, salidas, frecuencia/volumen y duración solo si fueron identificados.

### 5. Proceso futuro — TO-BE
Visión futura, cambios esperados, participación humana, resultado futuro y excepciones conocidas. Mantén nivel funcional.

### 6. Alcance
- Compañías: marca Sí/No solo conforme a lo elegido; si no se preguntó, deja vacío.
- Productos, tipo de cliente y marcas: solo lo elegido por el usuario.

### 7. Impacto esperado y KPIs
Beneficios cualitativos; beneficios cuantitativos solo con cifras aportadas; usuarios beneficiados; KPIs únicamente indicados o aceptados. Si no hay porcentaje, deja `+/-%` vacío.

### 8. Usuarios y áreas involucradas
Usuarios principales, área propietaria, áreas involucradas y terceros/proveedores confirmados.

### 9. Sistemas, datos y dependencias
Sistemas/herramientas, datos, integraciones conocidas, dependencias, restricciones y riesgos. No diseñes arquitectura ni inventes disponibilidad de integración.

### 10. Evaluación inicial y preclasificación
Incluye potencial de automatización, posible uso de IA, posible uso de agente, alternativa sin IA, complejidad percibida, un solo carril marcado `[X]` y justificación con motivo, dependencia principal y condiciones que podrían cambiar el carril.

### 11. Pendientes y siguientes pasos
Pendientes específicos, responsable sugerido solo si puede inferirse legítimamente como rol, momento, y siguiente paso congruente con el carril:
- Carril 1: Self-Service + capacitación configurada + Champion opcional.
- Carril 2: acompañamiento de Champion; tiempo variable por complejidad y carga/disponibilidad.
- Carril 3: IT/Data + backlog VWFS + evaluación/priorización + resumen ejecutivo del proceso corporativo. No es obligatorio copiar las 14 etapas completas dentro del Word salvo que sean necesarias para comprender la canalización.

## Regla absoluta del entregable
El Word final NO es:
- una plantilla vacía;
- una copia sin completar;
- un resumen del discovery;
- una lista de viñetas;
- un documento genérico;
- una versión simplificada;
- un documento con diseño alternativo.

El Word final ES la plantilla oficial completa, poblada con toda la información confirmada disponible, conservando portada, encabezado corporativo, tablas, secciones 1–11, tabla de carril y pie corporativo.

## Verificación final obligatoria
Antes de entregar, comprueba:
1. Se usó la plantilla oficial.
2. Están presentes las 11 secciones.
3. Se recorrió toda la conversación confirmada.
4. Los placeholders con información disponible fueron sustituidos.
5. Los datos faltantes quedaron vacíos y no fueron inventados.
6. `No aplica` solo aparece cuando el usuario lo indicó.
7. Solo un carril tiene `[X]`.
8. La justificación del carril coincide con las dependencias.
9. El siguiente paso coincide con el carril.
10. Para Carril 3, backlog/proceso corporativo se presentan sin prometer aprobación o fechas.
11. Problema → Objetivo → AS-IS → TO-BE → Impacto → Dependencias → Carril → Siguiente paso son coherentes.
12. Una persona que no participó en el discovery puede comprender la iniciativa leyendo el documento.

Si alguna comprobación falla, corrige el documento antes de entregarlo.