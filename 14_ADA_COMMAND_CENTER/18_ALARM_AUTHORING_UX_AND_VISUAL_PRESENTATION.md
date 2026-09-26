# ADA Command Center — Alarm Authoring UX and Deferred Visual Presentation

Estado: **DECISION AGREED / MANAGER UX IN PROGRESS / DELIVERY PRESENTATION PLANNED**

## 1. Autoridad y alcance

Registro del acuerdo de producto de 2026-09-24. Se contrastó con:

- Implementación `moragaga/atlanticus:main@7b61eaea463bab10a595166fa12d015e4c015c78`.
- Canonical `moragaga/atlanticus-cannonical:main@9fa862b8ae70b49f247bbfb9413ba44e14499498`, antes de integrar este documento.
- Decisions `moragaga/atlanticus-decisions:main@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`.

Este documento **conserva la información acordada para no perderla**, pero el foco de implementación inmediato es terminar y probar el Manager de Alarm Configuration. No autoriza implementar anticipadamente Live Delivery ni modificar el snapshot v3. Distinguir implementación de acuerdo y propuesta.

## 2. CURRENT / VERIFIED: configuración y ownership

El agregado editable y su snapshot publicados continúan siendo:

```text
AlarmConfiguration(rules, messages)
AlarmConfigurationSnapshot(configuration, tool_dependencies)
schema_version = 3
```

Cada `AlarmDefinition` identifica `AlarmIdentity(family_key, alarm_key)`, tiene color semántico y declara `visual_targets` con `tool_key`, `component_keys`, `subcomponents` y `process_projection_mode` opcional según el tipo de Tool. Los subcomponentes se referencian inequívocamente mediante `(owner_component_key, subcomponent_key)`.

El catálogo Tool confirmado es autoridad de `tool_key`, tipo, estructura, nombres y relaciones. La publicación congela la evidencia exacta que B.2 consumirá; no utilizar un catálogo Tool posterior para reinterpretar una revisión Alarm histórica.

Los stores local y Cosmos de proyección y su composición existen en `atlanticus:main`; **la integración con infraestructura Azure real y la verificación end-to-end de Cosmos/Blob siguen UNVERIFIED**. Las afirmaciones más antiguas que describen el adapter Cosmos como ausente requieren reconciliación editorial separada.

## 3. DECISION AGREED: familias derivadas y prioridad inmediata

- Las familias del editor son agrupaciones **derivadas** de `rules[].identity.family_key` y `messages[]` con `scope=FAMILY`. No crear una entidad `Family` persistente ni agregar un catálogo duplicado.
- Los mensajes `GLOBAL` se administran separadamente y pueden referenciarse desde cualquier familia.
- La creación guiada de una familia comienza con su primera regla o mensaje; una familia vacía no constituye dato durable.
- El foco actual es completar una interfaz operable bajo la identidad visual de Atlanticus, validar su persistencia y cerrar el Manager antes de iniciar Materialization, Runtime o Live Delivery.
- Reutilizar shell, tokens, componentes administrativos y controles genéricos de Atlanticus. Implementar sólo controles específicos del dominio Alarm en su pantalla; no crear un tema alternativo ni duplicar CSS genérico.

## 4. DECISION AGREED: destino visual y tipo de presentación

La configuración de cada alarma indica **dónde** puede representarse y qué elementos quedan afectados: Tool destino, componentes, subcomponentes, color y, exclusivamente para Process, modo `GENERIC` o `DISTRIBUTED`. La Web traduce estas referencias a su geometría y pinta los componentes/subcomponentes correspondientes.

El tipo de presentación se deriva del **tipo de Tool**, no de un selector libre por alarma:

| Tool kind | Tipo de presentación | Modo por visual target |
|---|---|---|
| `INTEGRATED_OPERATIONS` | `QUEUE_IN_QUEUE` | `process_projection_mode=None` |
| `PROCESS` | `CAROUSEL` | `GENERIC` o `DISTRIBUTED` |
| `STRATEGIC` | No definido | Sin proyección visual Alarm acordada |

`QUEUE_IN_QUEUE` y `CAROUSEL` son nombres para la estrategia de presentación futura. **No existen aún como campos nuevos del agregado ni como scheduler implementado**. No duplicarlos por Rule mientras el tipo de Tool sea suficiente para deducirlos.

Separar estrictamente:

```text
visual_targets + semantic color       -> contrato authored
B.2 + frozen exact Tool evidence      -> validación y resolución
Live Delivery presentation planning  -> elegibilidad y rotación temporal
Web                                   -> geometría, representación y coloreado
```

El destino visual **no equivale** a los destinos de escalamiento/routing: no condicionar uno al otro sin una decisión explícita.

## 5. DECISION AGREED: comportamiento futuro de CAROUSEL

Un destino `PROCESS` dispone de **seis posiciones normales** mientras no sea necesaria una posición distribuida.

- Si existe cero o una alarma con modo `DISTRIBUTED`, esa alarma participa en el flujo normal de seis posiciones.
- Si existen **dos o más** alarmas distribuidas elegibles, reservar **cinco posiciones normales y una posición distribuida** en la esquina, incluso cuando haya espacios normales desocupados.
- Con dos o más distribuidas, el conjunto distribuido rota independientemente para ocupar su única posición; las normales constituyen otra cola.
- Cuando hay más alarmas elegibles que posiciones normales visibles, las que esperan deben entrar mediante rotación: al vencer su tiempo, la primera sale del conjunto visible y avanza la siguiente. No rotar innecesariamente por mero paso de tiempo cuando no hay espera, salvo una regla posterior explícita.

**OPEN:** intervalos exactos, su origen/configuración, orden ante cambios de prioridad, tratamiento temporal de altas/bajas, sincronización de las dos colas, persistencia/reanudación de estado y contrato físico del snapshot temporal. No introducir defaults inventados en el Manager.

## 6. DECISION AGREED: comportamiento futuro de QUEUE_IN_QUEUE

`INTEGRATED_OPERATIONS` expone **tres posiciones Mina y tres posiciones Planta**. Los componentes elegibles de cada área pueden competir por sus tres posiciones y cada componente puede contener múltiples alarmas activas en espera.

Se requiere rotación en **dos niveles**:

1. Entre componentes con alarmas elegibles, para permitir visualizar componentes que quedaron fuera de las tres posiciones de su área.
2. Entre alarmas activas de un mismo componente, para mostrar las que esperan dentro de esa posición.

La política debe evitar que un componente o una alarma quede indefinidamente invisible por el avance de otras colas. El scheduler respeta las áreas y no inventa asociaciones de componentes: las referencias y relaciones proceden de la evidencia Tool exacta.

**OPEN:** regla determinista de fairness, orden de las colas, duración visible, interacción entre ambos niveles, refresco ante cambios, restauración tras reinicio y qué metadatos temporales recibe Web. No convertir aquí ninguna alternativa algorítmica en contrato congelado.

## 7. CURRENT/DECISION AGREED: frontera B.2, Delivery y Web

- B.2 conserva la correlación exacta de `AlarmResolutionKey` entre Runtime y Delivery; resuelve Tool kind y visual targets, pero no lleva reloj de carruseles ni calcula geometría UI.
- Live Delivery sólo presenta occurrences que Runtime y el contrato de publicación ya declaran publicables. `TRACE_ONLY`, `ECLIPSED` y `CASCADE_SUPPRESSED` no se promocionan por tener posiciones libres.
- La planificación temporal se diseñará **después** de completar el Manager, probar una alarma configurada y cerrar la cadena Materialization -> Runtime/Delivery -> Live.
- Web consume un contrato resuelto: no vuelve a resolver Tool Catalog, prioridad ni routing, y no interpreta `cause_template` como fuente autoritativa de texto.
- La versión antigua de la visualización Web será **referencia futura de integración, no autoridad de arquitectura**. Al volver a conectarla se verificará qué datos realmente requiere para posiciones, color y rotación.

El diseño de Live Delivery existente en `16_ALARM_LIVE_DELIVERY_CONTRACT.md` no queda reemplazado por este documento. Aquí se preserva la intención de presentación y se identifican extensiones aún no definidas.

## 8. IN PROGRESS: requisitos del Manager antes de cerrar

### Flujo de creación

- Familia derivada -> primera Rule/Message mediante formulario guiado y cancelable.
- Una regla incompleta en autoría **no debe mostrarse como fallo de una publicación**; exponer diagnóstico por campo/sección y diferenciar trabajo sin terminar de una configuración intrínsecamente válida.
- La creación de una familia existente debe dirigir al usuario a esa familia; no repetir botones inválidos ni producir errores genéricos.
- `alarm_key` es identidad estable y no equivale a `rule_name`: generación automática inicial **PROPOSED**, con revisión de unicidad antes de confirmar y sin regeneración automática tras renombrar. El algoritmo/instante de bloqueo de la identidad sigue OPEN.

### Presentación y validación guiada

- Interfaz y ayudas al configurador en español. Los nombres técnicos y valores serializados permanecen invariantes, pero se representan mediante etiquetas legibles.
- Editor compacto y navegable por familia, reglas, mensajes y **secciones** de una Rule; evitar un formulario gigante.
- Selección Tool -> Component -> Subcomponent guiada por el catálogo confirmado. Conservar la identidad técnica `(owner_component_key, subcomponent_key)` y mostrar nombres legibles.
- Un target `INTEGRATED_OPERATIONS` no ofrece `GENERIC/DISTRIBUTED`; un target `PROCESS` sí. `STRATEGIC` no se presenta como target admitido mientras no exista contrato.
- La petición de una única selección de componente para `PROCESS` es **PROPOSED/OPEN**: el contrato CURRENT permite múltiples `component_keys`; decidir y reconciliar antes de restringirlo o migrar documentos.
- C1: pasos de escalamiento inmediatos. C2: pasos habilitados con espera positiva. C3: origen únicamente, sin permitir configurar nuevos pasos habilitados. No eliminar pasos antiguos silenciosamente al cambiar criticidad.
- `is_special_condition` marca **esta** Rule; `reappearance.special_conditions` son referencias a **otras Rules especiales** de la misma familia y grupo que pueden disparar su reaparición. Son responsabilidades distintas y requieren ayudas claras.
- Crear parámetros desde la sección de evaluación sin exigir JSON manual si se dispone de controles genéricos; el contrato de valores permanece `str | float | bool`. No inventar el schema de cada evaluador antes de disponer de catálogo calificado.
- Controles de guardado coherentes con Atlanticus, detalle de revisión Tool/Source/Projection y errores localizados que indiquen qué corregir.

### Borradores y Manager genérico

- CURRENT: Alarm editor mantiene documento transitoriamente incompleto en su store de autoría, pero `Save local draft` hace `AlarmConfiguration.from_document()` y exige un documento íntegro; errores genéricos actuales impiden distinguir campos faltantes.
- CURRENT: el Manager genérico guarda el draft recuperable en almacenamiento local y ofrece recuperación explícita; no restaura automáticamente ese draft al abrir.
- **OPEN / requiere contrato previo:** guardar avances incompletos, recuperación automática sólo cuando sea compatible, tratamiento de Source divergente y responsabilidades entre el binding específico Alarm y el Manager genérico. No alterar la semántica genérica ni descartar drafts silenciosamente.

## 9. Condiciones de cierre del Manager

**PLANNED / requiere pruebas**:

1. Navegación y creación/edición de familias, reglas y mensajes, incluidos mensajes `GLOBAL` y condiciones especiales.
2. Identidades y referencias estables; sin pérdida de cambios al cambiar de sección, familia o elemento.
3. Validaciones de campos y dependencias útiles; distinción clara entre autoría incompleta, borrador guardado y publicación válida.
4. Selección asistida de herramientas, componentes, subcomponentes y restricciones por criticidad/tipo de herramienta.
5. UX Atlanticus validada visualmente en navegador, sin tests que congelen CSS, layout o detalles internos.
6. Guardar -> validar -> verificar Source -> publicar -> proyectar; comprobar `OUTDATED` cuando cambia Source y persistencia tras reiniciar.

Cerrar esto **antes** de ejecutar el frente separado de Materialization/Live Delivery. Hacer incrementos pequeños: primero contratos y UX de creación/edición, luego cambios de frontend, luego pruebas funcionales.

## 10. OPEN / conflictos que no se resuelven aquí

- `13_OPEN_ITEMS.md`, `02_CURRENT_IMPLEMENTATION.md`, `04_CONFIGURATION_SCOPE.md` y `06_ENGINE_AND_PROJECTIONS.md` se redactaron antes del adapter Cosmos actual. Este incremento actualiza algunos enlaces/estados, **pero el barrido completo de documentación del estado Cosmos/deployment es independiente**.
- La formulación histórica B.1 sobre Special Cascade difiere del tratamiento por prioridad vigente registrado en `04_ALARM_ENGINE/01_DOMAIN_MODEL.md`; no reabrir ni resolver implícitamente para mejorar el editor.
- El documento B.1 describe Process `GENERIC/DISTRIBUTED` como propiedad del **visual target de cada Rule**, no un modo de toda la familia. Preservarlo.
- No cambiar la versión del Source, el dominio, el materializador ni el consumidor Web para acomodar una maqueta.
