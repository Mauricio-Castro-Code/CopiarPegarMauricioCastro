# VWFS Initiative Template Guidelines

## Propósito

Este documento define cómo VWFS AI Opportunity Discovery Agent debe completar el Registro de Iniciativa oficial utilizando exclusivamente la información obtenida y validada durante discovery.

La plantilla oficial es la estructura de salida.

No modificar, eliminar, reordenar o crear secciones diferentes salvo que exista instrucción corporativa expresa.

---

# Reglas generales

- Utilizar únicamente información obtenida durante discovery.
- No inventar datos.
- No copiar ejemplos de Knowledge como información real.
- Mantener redacción ejecutiva, clara y breve.
- Evitar repetir la misma información en diferentes secciones.
- Distinguir hechos confirmados de pendientes.
- Utilizar "Pendiente de validar" cuando falte información.
- Utilizar "Pendiente de cuantificar" para métricas desconocidas.
- Utilizar "No aplica" únicamente cuando se haya determinado que un campo no corresponde.
- No realizar afirmaciones técnicas definitivas que no hayan sido validadas.

---

# Portada

## Nombre de la iniciativa

Utilizar el nombre validado con el usuario.

Si el agente propone el título, debe ser descriptivo y neutral respecto a tecnología cuando la tecnología todavía no esté confirmada.

No modificar campos corporativos como:

- Gerencia
- Clasificación
- Versión
- Vigente desde
- Título

salvo que exista una instrucción explícita para hacerlo.

## Historial de cambios

No inventar versiones, autores o comentarios.

Completar únicamente cuando el proceso definido permita determinar esos datos.

---

# 1. Datos generales

## Nombre de la iniciativa

Nombre validado.

## Área solicitante

Área proporcionada por el usuario.

## Solicitante

Nombre o rol del solicitante según la información disponible.

## Sponsor

Sponsor confirmado.

Si no existe todavía:

"Pendiente de validar."

## Fecha

Fecha correspondiente a la generación o registro, según el proceso corporativo definido.

---

# 2. Objetivo / Para qué

## Objetivo de negocio

Explicar qué pretende conseguir la iniciativa y por qué resulta relevante para el negocio.

No describir aquí la solución técnica.

## Resultado esperado

Describir qué debería mejorar o cambiar si la iniciativa funciona correctamente.

Evitar repetir literalmente el objetivo.

---

# 3. Problema actual

## Situación actual

Describir de forma ejecutiva el contexto actual.

## Principal dolor o necesidad

Identificar claramente el problema central.

## Consecuencias relevantes

Registrar efectos conocidos del problema.

No inventar causas ni consecuencias.

---

# 4. Proceso actual — AS-IS

## Flujo actual

Representar preferentemente:

Inicio → Actividades → Interacciones/decisiones → Resultado actual

El flujo debe poder comprenderse sin haber participado en la conversación.

## Actores

Usuarios, roles o áreas que participan actualmente.

## Herramientas

Sistemas, aplicaciones o herramientas utilizadas actualmente.

## Principales puntos de fricción

Registrar únicamente los identificados durante discovery.

Ejemplos:

- actividad manual;
- espera;
- duplicidad;
- error;
- retrabajo;
- falta de integración.

---

# 5. Proceso futuro — TO-BE

## Flujo futuro esperado

Representar:

Inicio → Nuevo flujo → Participación humana/automatizada → Resultado esperado

No diseñar arquitectura técnica.

## Participación humana

Describir actividades, validaciones, decisiones o supervisión humana esperada.

## Participación automatizada

Describir funcionalmente las actividades que podrían automatizarse o recibir asistencia.

No afirmar que una tecnología específica será utilizada salvo que esté validada.

## Resultado futuro

Resultado funcional esperado del nuevo proceso.

---

# 6. Alcance de la iniciativa

## Empresas impactadas

Marcar Sí o No exclusivamente según información validada:

- VW Servicios
- VW Leasing
- VW Bank
- VW IB

No dejar marcada una empresa por inferencia.

## Productos y perfiles de clientes impactados

Registrar únicamente los identificados.

Si no aplica:

"No aplica."

## Marcas impactadas

Registrar VW, SEAT u otras cuando corresponda.

Si no existe impacto específico por marca:

"No aplica."

## Usuarios y áreas involucradas

Registrar usuarios, roles, equipos o áreas relevantes.

Evitar incluir personas o áreas solamente porque podrían participar posteriormente.

---

# 7. Impacto esperado y contribución a KPIs

## Principales beneficios esperados

Resumir los beneficios más relevantes.

Preferir beneficios concretos y vinculados al problema.

## Tabla de KPIs

### KPI

Registrar el indicador identificado.

### Impacto esperado (+/- %)

Utilizar la cifra proporcionada por el usuario cuando exista.

Si se espera impacto pero todavía no está cuantificado:

"Pendiente de cuantificar."

Nunca inventar porcentajes.

### Justificación

Explicar brevemente por qué la iniciativa podría contribuir al KPI.

### Área

Registrar el área relacionada con el KPI cuando se conozca.

Si no se conoce:

"Pendiente de validar."

No es obligatorio llenar todas las filas disponibles.

---

# 8. Sistemas, datos y dependencias

## Sistemas / herramientas involucrados

Registrar sistemas actuales y, cuando esté claramente identificado, capacidades necesarias para el futuro.

Diferenciar entre existente y potencial cuando sea necesario.

## Fuentes de información o datos

Registrar fuentes identificadas durante discovery.

## Dependencias relevantes

Registrar dependencias que puedan afectar la viabilidad o canalización.

## Integraciones o accesos requeridos

Registrar integraciones, APIs, conectores, MCP, permisos o accesos únicamente cuando hayan sido identificados.

Si todavía debe comprobarse su disponibilidad, indicarlo explícitamente.

---

# 9. Evaluación inicial y preclasificación

## Evaluación preliminar

Seleccionar una sola opción por criterio.

### Potencial de automatización

Alto / Medio / Bajo / Requiere análisis

### Posible uso de IA

Sí / No / Requiere análisis

### Posible uso de agente

Sí / No / Requiere análisis

### Alternativa sin IA

Sí / No / Requiere análisis

Aplicar los criterios definidos en Knowledge.

No forzar una evaluación favorable hacia IA.

## Preclasificación

Consultar obligatoriamente VWFS Classification Guidelines.

Seleccionar exclusivamente uno:

- Carril 1 — Self-Service
- Carril 2 — Champion Assisted
- Carril 3 — IT Assisted / Integration

Si todavía no existe suficiente información para determinarlo, no inventar una selección. Registrar el pendiente correspondiente antes de generar la versión definitiva, según el proceso permitido.

## Justificación de la preclasificación

Explicar brevemente:

- por qué corresponde ese carril;
- cuál es la dependencia principal;
- qué condición podría modificar la clasificación si aplica.

La justificación debe basarse en la información de las secciones anteriores.

No utilizar complejidad percibida como criterio independiente.

La clasificación es preliminar y no representa aprobación técnica.

---

# 10. Pendientes y siguiente paso

## Pendientes por validar

Listar únicamente pendientes relevantes para continuar.

Cada pendiente debe ser concreto.

Ejemplo:

"Confirmar si el sistema X dispone de un conector corporativo autorizado."

No utilizar frases genéricas como:

"Revisar tema técnico."

## Siguiente paso recomendado

Debe corresponder al carril seleccionado.

### Carril 1

Continuar mediante capacidades self-service corporativas disponibles.

### Carril 2

Continuar con acompañamiento de Champions para configuración o implementación.

### Carril 3

Continuar con evaluación de IT sobre integraciones, accesos, datos o dependencias identificadas.

No prometer aprobación o implementación.

## Preclasificación final visible

El valor mostrado al final del documento debe coincidir exactamente con el carril seleccionado en la sección 9.

---

# Validación previa a generación

Antes de completar el documento, verificar internamente:

- Nombre de iniciativa consistente.
- Objetivo y resultado esperado diferenciados.
- Problema claramente explicado.
- AS-IS coherente con el problema.
- TO-BE coherente con el objetivo.
- Empresas, productos, marcas y usuarios basados en información real.
- KPIs no inventados.
- Sistemas y dependencias no asumidos.
- Evaluación coherente con el discovery.
- Carril coherente con VWFS Classification Guidelines.
- Justificación coherente con el carril.
- Pendientes explícitos.
- Siguiente paso coherente con la preclasificación.

Esta validación es interna.

No crear un anexo de cobertura en el documento final.

---

# Regla final

El Registro de Iniciativa debe permitir que una persona que no participó en el discovery comprenda:

Problema → Proceso actual → Proceso futuro → Alcance → Impacto → Dependencias → Preclasificación → Siguiente paso

El objetivo es facilitar evaluación y canalización, no realizar un assessment técnico definitivo.