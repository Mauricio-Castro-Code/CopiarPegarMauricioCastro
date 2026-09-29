Quiero que modifiques DIRECTAMENTE el documento Word adjunto para convertirlo en el formato oficial de salida de un agente llamado "VWFS AI Opportunity Discovery Agent".

IMPORTANTE:
- Debes EDITAR el archivo Word existente, no crear una plantilla completamente diferente.
- Conserva el estilo visual, identidad, encabezados, colores, tablas y formato corporativo del documento original siempre que sea posible.
- Simplifica su contenido y estructura.
- El resultado debe ser una ficha ejecutiva de iniciativa, no un cuestionario.
- No llenes el documento con información de una iniciativa ficticia.
- Déjalo como PLANTILLA VACÍA lista para ser completada posteriormente por el agente.
- No agregues información que no esté solicitada en estas instrucciones.

## CONTEXTO

VWFS AI Opportunity Discovery Agent realiza una conversación previa con el colaborador.

Durante esa conversación obtiene:
- objetivo;
- problema;
- proceso actual;
- proceso futuro;
- impacto esperado;
- usuarios y áreas;
- empresas impactadas;
- productos y perfiles de clientes impactados;
- marcas impactadas;
- KPIs;
- sistemas;
- datos;
- dependencias;
- sponsor;
- riesgos o restricciones;
- información necesaria para una evaluación preliminar;
- preclasificación Carril 1, 2 o 3.

Por lo tanto, el Word NO debe hacer preguntas al colaborador.

Debe servir como resumen estructurado del discovery realizado por el agente.

## OBJETIVO DE LA MODIFICACIÓN

Reduce y reorganiza el documento actual para que una persona pueda entender rápidamente:

1. Qué iniciativa se propone.
2. Para qué se necesita.
3. Qué problema resuelve.
4. Cómo funciona el proceso hoy.
5. Cómo funcionaría con la iniciativa.
6. Cuál es su alcance.
7. Qué impacto y KPIs tendría.
8. Qué sistemas, datos y dependencias existen.
9. Qué evaluación preliminar realizó el agente.
10. A qué carril fue preclasificada y por qué.
11. Qué queda pendiente y cuál es el siguiente paso.

## ESTRUCTURA DESEADA

Reorganiza el documento utilizando estas secciones:

### 1. Datos generales

Incluir campos para:
- Nombre de la iniciativa
- Área solicitante
- Solicitante
- Sponsor
- Fecha

Mantén otros datos generales del formato original únicamente si son necesarios para identificar o canalizar la iniciativa.

### 2. Objetivo / Para qué

Espacio para describir brevemente:
- objetivo de negocio;
- resultado esperado.

### 3. Problema actual

Espacio para:
- situación actual;
- principal dolor o necesidad;
- consecuencias relevantes.

Evita duplicar información del objetivo.

### 4. Proceso actual — AS-IS

Espacio para explicar cómo funciona actualmente el proceso.

Debe permitir representar de forma sencilla:

Inicio → actividades → interacciones/decisiones → resultado actual.

Debe poder incluir actores, herramientas y principales puntos de fricción.

### 5. Proceso futuro — TO-BE

Espacio para explicar cómo se espera que funcione el proceso con la iniciativa.

Debe permitir representar:

Inicio → nuevo flujo → participación humana/automatizada → resultado esperado.

No debe solicitar arquitectura técnica.

### 6. Alcance de la iniciativa

Incluir:

#### Empresas impactadas

Conservar o adaptar la tabla existente para indicar Sí/No en:
- VW Servicios
- VW Leasing
- VW Bank
- VW IB

#### Productos y perfiles de clientes impactados

Crear un campo sencillo para registrar productos y perfiles afectados.

#### Marcas impactadas

Crear un campo para registrar VW, SEAT u otras marcas cuando aplique.

#### Usuarios y áreas involucradas

Incluir los principales usuarios, equipos o áreas participantes.

### 7. Impacto esperado y contribución a KPIs

Incluir un breve espacio para describir los principales beneficios esperados.

Conservar/adaptar la tabla de KPIs con:

- KPI
- Impacto esperado (+/- %)
- Justificación
- Área

El porcentaje puede quedar como "Pendiente de cuantificar" cuando todavía no exista información.

No obligar a disponer de una cifra para poder registrar la iniciativa.

### 8. Sistemas, datos y dependencias

Crear una sección compacta para registrar:
- sistemas/herramientas involucrados;
- fuentes de información o datos;
- dependencias relevantes;
- integraciones o accesos que podrían requerirse.

No convertir esta sección en un assessment técnico, de arquitectura o seguridad.

### 9. Evaluación inicial y preclasificación

Mantener una evaluación preliminar compacta que permita registrar:

- Potencial de automatización: Alto / Medio / Bajo / Requiere análisis.
- Posible uso de IA: Sí / No / Requiere análisis.
- Posible uso de agente: Sí / No / Requiere análisis.
- Alternativa sin IA: Sí / No / Requiere análisis.

Eliminar "Complejidad percibida" si no es necesaria para la canalización inicial.

Después incluir:

### Preclasificación

Mantener los tres carriles:

Carril 1 — Self-Service
Capacidades corporativas ya disponibles para el colaborador.

Carril 2 — Champion Assisted
Requiere acompañamiento de Champions para configuración o implementación.

Carril 3 — IT Assisted / Integration
Requiere evaluación de IT por integraciones, APIs, MCP, acceso a datos, infraestructura, desarrollo u otras dependencias técnicas.

Debe existir un campo:
"Justificación de la preclasificación"

La clasificación es preliminar y no representa aprobación técnica.

### 10. Pendientes y siguiente paso

Simplifica la sección actual.

Incluir:

#### Pendientes por validar
Lista breve de información relevante que todavía debe confirmarse.

#### Siguiente paso recomendado
Acción correspondiente a la preclasificación.

No es necesario incluir responsables sugeridos o momentos ("Ahora", "Evaluación posterior") salvo que ya formen parte obligatoria del formato corporativo original.

## ELEMENTOS A ELIMINAR O SIMPLIFICAR

Elimina redundancias entre:
- resumen ejecutivo;
- objetivo;
- problema;
- impacto;
- observaciones finales.

No necesitamos repetir la misma información en diferentes secciones.

ELIMINA del documento entregable el apartado:

"ANEXO. Validación de cobertura del formato oficial"

Esa validación debe realizarse internamente y no aporta valor al usuario final.

Elimina también textos explicativos internos destinados al diseño del agente, instrucciones al agente o notas como:
- "qué le pregunte..."
- "al último que confirme..."
- instrucciones sobre cómo realizar el discovery.

Esas reglas pertenecen a las instrucciones del agente, no al formato final.

Mantén únicamente información que forme parte del entregable de la iniciativa.

## CRITERIOS DE DISEÑO

El documento final debe:
- ser ejecutivo;
- ser fácil de leer;
- evitar redundancias;
- mantener apariencia corporativa;
- utilizar tablas solamente cuando faciliten la lectura;
- tener suficiente espacio para respuestas sin generar páginas innecesarias;
- diferenciar claramente AS-IS y TO-BE;
- hacer visible rápidamente el carril asignado;
- idealmente mantenerse compacto.

No reduzcas contenido simplemente para disminuir páginas. Prioriza claridad y utilidad.

## MUY IMPORTANTE

No agregues un caso de ejemplo.

No llenes los campos.

No inventes información.

No cambies innecesariamente la identidad visual corporativa.

No conviertas el documento en un formulario de preguntas.

EDITA el archivo Word original y entrégame el documento modificado.

Antes de finalizar, verifica que no haya secciones duplicadas y que todas las tablas conserven un formato legible y consistente.