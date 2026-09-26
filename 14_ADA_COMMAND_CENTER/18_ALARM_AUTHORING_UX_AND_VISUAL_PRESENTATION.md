# ADA Command Center — Alarm Authoring UX and Deferred Visual Presentation

Estado: **AUTHORING/Routing COMPONENTS IMPLEMENTED; HOST/E2E UNVERIFIED; DELIVERY PRESENTATION AGREED/PLANNED**

## 1. Autoridad y alcance

Acuerdo inicial de producto de 2026-09-24 y refinamiento de routing del 2026-09-26, contrastados con:

```text
moragaga/atlanticus@411aea44ac60c09d2b07ce41d34c3f378788b97b
moragaga/atlanticus-cannonical@83cd871c8418e37d2c29dff30e2ea5ef54bda4a0 (antes del reemplazo)
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

Este documento conserva el acuerdo visual futuro para no perderlo, **no autoriza** implementar Live Delivery, un scheduler, esquema nuevo ni cambiar Source v3. Las suites locales del editor y de B.2 están verdes según evidencia del usuario; no equivalen a aceptación visual en navegador ni E2E posterior al routing.

## 2. CURRENT: configuración y ownership

```text
AlarmConfiguration(rules, messages)
AlarmConfigurationSnapshot(configuration, tool_dependencies)
schema_version = 3
```

Cada Rule mantiene `AlarmIdentity(family_key, alarm_key)`, color semántico y `visual_targets` con `tool_key`, `component_keys`, `subcomponents` y, para Process, `process_projection_mode`. La identidad de cada subcomponent es `(owner_component_key, subcomponent_key)`. El Tool Catalog confirmado es autoridad de tool key/kind/estructura; cada nueva publicación congela evidencia exacta Tool Cn.

Source/base projection, stores Local/Cosmos y su composición existen en código. La integración real Blob/Cosmos y la conexión operacional del productor todavía son **UNVERIFIED**. No afirmar que el proceso B.2 existe: únicamente su pure resolver está implementado.

## 3. Familias derivadas, prioridad y experiencia — CURRENT/DECIDED

- Las familias derivan de `rules[].identity.family_key` y Messages `scope=FAMILY`, sin entidad `Family` durable ni catálogo duplicado.
- Los Messages `GLOBAL` se administran separadamente y se pueden referenciar desde cualquier familia.
- Una nueva familia nace durable con su primera Rule/Message; no persistir familias vacías.
- Reutilizar shell, tokens, estilos administrativos y controles Atlanticus. No crear tema propio o CSS duplicado para Alarm.
- Las tarjetas de familias mantienen acciones Administrar/Eliminar coherentes con el resto; las tarjetas de reglas incluyen grupo y ranking. Se corrigió el campo Nueva familia para reutilizar el estilo común del formulario. **Estos comportamientos se verificaron por suites de componente, no por aceptación visual final documentada.**

## 4. Strict routing — DECIDED / IMPLEMENTED

```text
PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC -> END
```

**Sin excepciones:** no same-tier, retroceso ni saltar un nivel (incluido Process -> Strategic). Cada paso habilitado avanza exactamente al siguiente nivel respecto del último habilitado; un disabled step no autoriza saltarse el nivel. Ningún nivel intermedio es obligatorio si no existe destino: C1 y C2 pueden terminar en origen conservando criticidad. C3 usa sólo origen.

- C1: todos los destinos habilitados inmediatos.
- C2: espera entera positiva por cada paso habilitado; B.2 suma los waits en orden y Core programa deadlines relativos al inicio de la ocurrencia. `20 + 20 = minutos 20 y 40`.
- C3: sin nuevos pasos habilitados; los anteriores incompatibles se mantienen para corrección, nunca borrado silencioso.
- Source de autoridad de kind es la evidencia Tool exacta. Domain, B.2 y Web comparten la función `next_routing_tool_kind`.
- Strategic puede ser destino de routing desde Integrated Operations o el origen final, pero no posee presentación visual Alarm contratada.

## 5. DECISION AGREED: destino visual y tipo de presentación

Visual describe **dónde** y qué componentes/subcomponentes se representan; color semántico y Process GENERIC/DISTRIBUTED son propiedades authored. El tipo futuro de presentación se deriva del Tool kind, no de un selector por Rule:

| Tool kind | Estrategia futura | `process_projection_mode` |
|---|---|---|
| `INTEGRATED_OPERATIONS` | `QUEUE_IN_QUEUE` | `None` |
| `PROCESS` | `CAROUSEL` | `GENERIC` o `DISTRIBUTED` |
| `STRATEGIC` | No definida | Sin visual target Alarm admitido |

`CAROUSEL` y `QUEUE_IN_QUEUE` **no** son campos del Source v3 ni código de scheduler implementado.

```text
visual_targets + color            -> authored contract
B.2 + frozen Tool evidence        -> qualification/resolution
future Live Delivery planner      -> temporal eligibility/rotation
Web                              -> concrete geometry/painting
```

**CONFLICT DOCUMENTAL/CONTRACTUAL pendiente:** el acuerdo de esta sección establece que visual target no equivale a destinos de routing y no debe condicionarse sin una decisión explícita. En `main@411aea...`, `synchronize_visual_targets` deriva visual targets del origen y routing habilitado, excluyendo Strategic. Registrar esa diferencia, sin inferir una nueva decisión ni modificar el job para resolverla.

## 6. DECISION AGREED: CAROUSEL futuro para Process

- Process dispone de **seis posiciones normales** mientras no se necesite posición distribuida.
- Con cero o una alarma `DISTRIBUTED` elegible, participa en el flujo normal de seis posiciones.
- Con **dos o más** distribuidas elegibles, reservar **cinco posiciones normales y una distribuida**; mantener la reservada aun con posiciones normales vacías.
- El conjunto distribuido rota independientemente sobre su única posición; el conjunto normal constituye otra cola.
- Si hay más alarmas normales elegibles que slots visibles, las que esperan entran mediante rotación. No rotar innecesariamente por mera cadencia cuando no hay espera, salvo decisión posterior.

**OPEN:** intervalos, fuente de configuración, prioridad al entrar/salir, tratamiento temporal de altas/bajas, sincronía de colas, reanudación tras restart, codec y shape físico temporal. No inventar defaults.

## 7. DECISION AGREED: QUEUE_IN_QUEUE futuro para Integrated Operations

Integrated Operations expone **tres posiciones Mina y tres Planta**. Los componentes elegibles de cada área compiten por sus tres posiciones; cada componente puede contener varias alarmas activas en espera.

Rotación en **dos niveles**:
1. Entre componentes elegibles para evitar invisibilidad permanente fuera de las tres posiciones de su área.
2. Entre alarmas activas del mismo componente para mostrar las que esperan dentro de su posición.

Fairness, orden determinista de colas, duración visible, interacción entre niveles, refresh y recuperación siguen **OPEN**. Componentes, vínculos y scope proceden de frozen Tool evidence; no inventar nuevas asociaciones.

## 8. Frontera B.2 / Delivery / Web — CURRENT + LATER

- `AlarmResolutionKey` correlaciona Runtime y Delivery; B.2 conoce Tool kind y visual targets, **no** planifica carruseles ni geometría.
- Live Delivery sólo podrá exponer lo que Core y contrato de publicación consideren visible: no promover `TRACE_ONLY`, `ECLIPSED` ni `CASCADE_SUPPRESSED` por disponibilidad de posiciones.
- Web consumirá contrato resuelto, sin releer latest Tool Catalog, recalcular prioridad/routing ni tratar `cause_template` como texto autoritativo.
- La visualización Web histórica será referencia de requisitos concretos, **no autoridad arquitectónica**.
- `16_ALARM_LIVE_DELIVERY_CONTRACT.md` conserva su propio ámbito; este documento no lo reemplaza.

El siguiente job B.2 no debe implementar estas políticas temporales. El scheduler de presentación es posterior a Materialization, Runtime y Live Delivery.

## 9. Editor: hecho, pendiente y no decidido

**CURRENT / IMPLEMENTED / COMPONENT TESTS GREEN:** editor por familia/regla/mensaje y secciones, tarjetas consistentes, input Nueva familia corregido, ayudas y controles dinámicos, selección Tool->Component->Subcomponent, política estricta y separación `routing_tools`/visual `tools`, diagnóstico del estado de autoría y persistencia del contrato en sus tests correspondientes.

**UNVERIFIED en este corte:** prueba completa del host y navegador posterior al routing, UX responsiva real, cadena save/validate/publish/operational Cosmos desde infraestructura real y recuperación tras restart en esa integración.

**OPEN / requiere evidencia/decisión, no inventar implementación:**
- generación/instante exacto de congelación de `alarm_key` si se modifica la política actual;
- restricción propuesta de un único `component_key` en Process: el contrato vigente acepta múltiples; no restringir ni migrar documentos preventivamente;
- mejoras a guardado/recovery de drafts incompletos y conflicto Source/Manager genérico, sin cambiar semántica genérica;
- reconciliar la independencia visual/routing con la sincronización CURRENT del editor;
- catálogo de evaluator calificado y hints por schema, sólo cuando exista productor concreto.

`is_special_condition` marca esta Rule; `reappearance.special_conditions` referencia otras Rules especiales de la misma familia/grupo. No confundirlas al integrar.

## 10. Cierre de foco y siguiente etapa

El trabajo de diseño/implementación del routing estricto y su prueba de componentes está **CLOSED** para este chat. La aceptación host/browser y E2E permanece **UNVERIFIED**, no se declara falsamente terminada.

Siguiente foco único solicitado: job/backend de Materialization; antes de codificar, auditar las fronteras existentes de proyección operacional, qualification y artifact stores. Evitar mezclar adopción, Live Delivery, Management Capture o agenda visual.

## 11. Conflictos de documentación que sobreviven al hito

- Algunos documentos históricos todavía dicen que no existe adapter Cosmos; esa descripción está SUPERSEDED para el **adapter**, pero no prueba disponibilidad de Cosmos real ni un productor operacional.
- La formulación B.1 histórica de Special Cascade puede diferir del tratamiento por prioridad del Core actual: no resolver implícitamente durante un incremento de configuración.
- Para Process, `process_projection_mode` pertenece a cada visual target de una Rule, no a la familia completa.
- Project usa Python 3.14.7 mientras varios paquetes Command Center exigen 3.14.2. Mantener OPEN / SEPARATE.
