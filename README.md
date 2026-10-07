# classify_vwfs_initiative

## Propósito
Preclasificar una iniciativa de VWFS en Carril 1, 2, Carril 3 o Pendiente de validación, identificar la dependencia dominante y devolver una canalización accionable. La clasificación es preliminar; no representa aprobación, prioridad, valor ni compromiso de implementación.

## Cuándo usar esta skill
Úsala cuando el discovery tenga información suficiente para evaluar capacidades, sistemas, datos, accesos, integraciones y soporte requerido; y nuevamente si el usuario modifica información que pueda cambiar el carril.

## Fuentes obligatorias
- `1_Classification_Guidelines.docx`: fuente normativa principal para criterios, árbol de decisión, dependencia dominante, casos frontera y proceso corporativo de Carril 3.
- `3_Discovery_Guidelines.docx`: contexto sobre qué información debe haberse descubierto.
- Configuración editable de `Instrucciones_Agente.txt`: links y contactos variables.

Si existe conflicto, no inventes una regla nueva. Aplica primero las reglas explícitas de Classification Guidelines y señala la ambigüedad como pendiente.

## Entradas esperadas
Usa únicamente información confirmada en el discovery:
- objetivo y problema;
- AS-IS y TO-BE;
- usuarios/áreas;
- sistemas y herramientas;
- origen y uso de datos;
- lectura vs. escritura/modificación de sistemas;
- conectores, APIs, MCP o integraciones conocidas;
- accesos/permisos;
- desarrollo o infraestructura requerida;
- dependencias no confirmadas.

## Procedimiento
1. Identifica qué necesita hacer realmente la solución; no clasifiques por palabras como IA, agente, datos, SharePoint, automatización o conector.
2. Pregunta internamente si la iniciativa puede realizarse completamente con capacidades estándar ya habilitadas para el colaborador.
   - Si sí, evalúa Carril 1.
3. Si no, determina si las capacidades necesarias ya existen en el ecosistema autorizado pero requieren configuración, diseño o acompañamiento especializado.
   - Si sí y no existe una dependencia técnica que obligue a IT, evalúa Carril 2.
4. Determina si requiere una nueva integración o capacidad técnica: APIs corporativas/no disponibles, desarrollo de APIs, conectores custom, MCP especializado, acceso especializado a bases de datos, escritura en sistemas, backend, infraestructura, autenticación/autorización especializada, legacy u otra dependencia que requiera IT/Data.
   - Si sí, evalúa Carril 3.
5. Si falta información esencial para resolver un punto anterior, no escales por incertidumbre. Usa `Pendiente de validación` e identifica exactamente qué debe confirmarse.
6. Si existen componentes de varios carriles, aplica la regla de dependencia dominante: clasifica según la dependencia necesaria para que la iniciativa completa funcione.
7. Distingue consultar información de ejecutar acciones. La lectura puede permanecer en Carril 1/2 si existen capacidades aprobadas; una acción que requiera integración especializada puede llevar a Carril 3.
8. Genera la salida obligatoria descrita abajo.

## Salida obligatoria
Devuelve siempre:

**Preclasificación:** Carril X — Nombre / Pendiente de validación  
**Motivo:** explicación breve basada en lo descubierto.  
**Dependencia principal:** dependencia dominante o “No se identificaron dependencias adicionales”.  
**Podría cambiar si:** condición relevante, solo cuando aplique.  
**Siguiente paso:** canalización correspondiente.

## Canalización
### Carril 1 — Self-Service
- Indica que el colaborador puede avanzar principalmente con capacidades corporativas ya habilitadas.
- Recomienda los recursos configurados de Prompting, Intranet/capacitación y Self-Service si contienen valores reales.
- Si requiere orientación, puede solicitar apoyo al Champion de su área usando el contacto/instrucción configurados.
- El apoyo del Champion es opcional y no cambia por sí mismo el carril.

### Carril 2 — Champion Assisted
- Orienta al usuario al Champion correspondiente para estructurar, configurar, desarrollar o implementar usando capacidades corporativas existentes.
- Usa los datos de contacto/proceso configurados solo si son reales.
- Aclara que el tiempo de desarrollo/implementación depende de la complejidad y de la carga o disponibilidad del Champion.
- No prometas fechas ni disponibilidad inmediata.

### Carril 3 — IT Assisted / Integration
- Explica la dependencia técnica/de datos que obliga a participación de IT y/o Data.
- Indica que la iniciativa debe incorporarse al backlog correspondiente de VWFS para evaluación, priorización y atención conforme a la metodología corporativa.
- Usa el link de backlog solo si está configurado con un valor real.
- No prometas aprobación, prioridad, recursos ni fechas.
- Presenta el proceso corporativo general de 14 etapas cuando cierres la iniciativa o cuando el usuario pregunte por el camino de implementación:
  1. Requerimiento / necesidad / idea de negocio.
  2. Análisis de requerimientos, factibilidad, estimación y justificación.
  3. Presentación y autorización en Foro de IA.
  4. Diseño de solución y planeación.
  5. Validación de Arquitectura y Seguridad del Diseño de Solución (obtención de Visto Bueno).
  6. Desarrollo en DEV con control de versiones (Soluciones).
  7. Pruebas unitarias y de integración.
  8. Promoción a TEST (UAT, seguridad, rendimiento).
  9. Pruebas de Negocio y Protocolo de pruebas.
  10. Quality Board, Release Board.
  11. Documentación IDP.
  12. Aprobación de cambio (CAB).
  13. Liberación a PROD con soluciones administradas.
  14. Monitoreo, soporte y mejora continua.
- Las 14 etapas describen el recorrido general. No afirmes que una etapa fue iniciada, aprobada o completada sin confirmación explícita. No todas son ejecutadas por el solicitante.

### Pendiente de validación
Indica exactamente qué dato debe confirmarse y, si es posible, cómo afectaría al carril. Ejemplo de estructura: “Confirmar si X dispone de un conector corporativo aprobado. Si existe y solo requiere configuración podría permanecer en Carril 2; si requiere una integración nueva podría pasar a Carril 3.”

## Restricciones
- No favorezcas un carril.
- Carril 1 no significa menor importancia.
- Carril 3 no significa mayor valor o sofisticación.
- No conviertas la preclasificación en aprobación técnica.
- No inventes sistemas, conectores, permisos, contactos, responsables, fechas ni decisiones.