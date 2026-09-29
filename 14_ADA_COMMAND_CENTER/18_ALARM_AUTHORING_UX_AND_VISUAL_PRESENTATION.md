# ADA Command Center — Alarm Authoring UX and Deferred Visual Presentation

Estado: **AUTHORING/Routing COMPONENTS IMPLEMENTED; C1 WEB TOOL LIBRARIES CLOSED ESTRUCTURALMENTE; HOST/BROWSER E2E Y ACEPTACIÓN VISUAL UNVERIFIED; DELIVERY PRESENTATION AGREED/PLANNED**. El acuerdo visual y de routing histórico se conserva sin convertirlo en código de C1. Último checkpoint de ownership Web verificado `atlanticus:main@3961385aecd0eb7e373018fc25e509a71dccc409`.

## 1. Autoridad y alcance

Acuerdo inicial de producto de 2026-09-24 y refinamiento de routing de 2026-09-26, contrastados históricamente con:

```text
moragaga/atlanticus@411aea44ac60c09d2b07ce41d34c3f378788b97b
moragaga/atlanticus-cannonical@83cd871c8418e37d2c29dff30e2ea5ef54bda4a0
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

C1 sólo reubicó Tool Catalog/Discovery a Web y consolidó Tool Catalog Manager como biblioteca UI independiente. No modifica el acuerdo visual futuro ni autoriza implementar Live Delivery, scheduler, schema nuevo o cambiar Source v3. Las suites de componente/Engine no sustituyen aceptación en navegador.

## 2. CURRENT: configuración y ownership

```text
AlarmConfiguration(rules, messages)
AlarmConfigurationSnapshot(configuration, tool_dependencies)
schema_version = 3
```

Cada Rule conserva `AlarmIdentity(family_key, alarm_key)`, color semántico y `visual_targets` con `tool_key`, `component_keys`, `subcomponents` y, para Process, `process_projection_mode`. Cada subcomponent se identifica con `(owner_component_key, subcomponent_key)`. Tool Catalog confirmado es autoridad de Tool key/kind/estructura; cada release Alarm congela la evidencia exacta Cn.

Source/base projection, stores Local/Cosmos y composición de Alarm Configuration existen en código; integración física Blob/Cosmos E2E todavía es **UNVERIFIED** en el corte C1. **Actualización de estado frente a redacción histórica:** Materialization no es sólo pure resolver futuro; el job de publicación READY/BLOCKED ya existe antes de C1. Esto no acredita un producer de Qualification real ni despliegue durable.

**Owner Tool CURRENT C1:** `web/tools/catalog` almacena y consolida; `web/tools/discovery-cosmos` inspecciona/confirm; `web/tools/catalog-manager` posee UI/callbacks. Ninguno debe restaurarse en `backend/tools` por estar escrito en Python. La normalización transversal de `domain/tools` respecto a estructuras `ada-web-tools` continúa diferida.

## 3. Familias, prioridad y experiencia — CURRENT/DECIDED

- Familias derivan de `rules[].identity.family_key` y Messages `scope=FAMILY`, sin Family durable duplicada.
- Messages `GLOBAL` se administran aparte y pueden referenciarse desde familias.
- Nueva familia nace durable con primera Rule/Message; no persistir familias vacías.
- Reutilizar shell, tokens, estilos y controles Atlanticus; no crear tema propio/CSS duplicado para Alarm.
- Tarjetas de familia mantienen Administrar/Eliminar; tarjetas de Rules contienen grupo/ranking; campo Nueva familia ya usa estilo común. La evidencia histórica es de pruebas de componente, no aceptación visual definitiva.

## 4. Strict routing — DECIDED / IMPLEMENTED

```text
PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC -> END
```

Sin misma categoría, retroceso ni saltos, incluyendo PROCESS→STRATEGIC. Cada paso habilitado avanza exactamente un nivel respecto al último habilitado; un disabled step no da permiso de salto. C1/C2 pueden quedar en origin sin tocar criticality; C3 usa sólo origin.

- C1: destinos habilitados inmediatos.
- C2: espera entera positiva por step habilitado; B.2 suma waits en orden y Core programa deadlines relativos al inicio de occurrence. `20 + 20 = minuto 20 y minuto 40`.
- C3: sin steps habilitados; configuración anterior incompatible permanece visible para corrección, nunca se borra silenciosamente.
- La autoridad de kind es Tool evidence exacta congelada. Domain, B.2 y Web usan `next_routing_tool_kind`.
- Strategic puede ser destino de routing desde Integrated Operations o el último origin, pero NO posee visualización Alarm contratada.

## 5. DECISION AGREED: destino visual y estrategia futura

Visual expresa dónde y qué componentes/subcomponentes representar, con color semántico y modo Process GENERIC/DISTRIBUTED authored. La estrategia temporal futura depende del Tool kind, no de selector por Rule:

| Tool kind | Estrategia futura | `process_projection_mode` |
|---|---|---|
| `INTEGRATED_OPERATIONS` | `QUEUE_IN_QUEUE` | `None` |
| `PROCESS` | `CAROUSEL` | `GENERIC` o `DISTRIBUTED` |
| `STRATEGIC` | No definida | Visual target Alarm no permitido |

`CAROUSEL` y `QUEUE_IN_QUEUE` NO son campos Source v3 ni scheduler ya implementado.

```text
visual_targets + color            -> authored contract
B.2 + frozen Tool evidence        -> qualification/resolution
future Live Delivery planner      -> temporal eligibility/rotation
Web                              -> concrete geometry/painting
```

**CONFLICT abierto:** este acuerdo conceptual diferencia visual targets de routing y no permite condicionarlos sin una decisión. La implementación histórica `synchronize_visual_targets` deriva targets desde origin+routing habilitado excluyendo Strategic. Registrar ambas realidades sin suponer un cambio contractual aprobado ni aprovechar C2 para modificarlo.

## 6. DECISION AGREED: CAROUSEL futuro para Process

- Process tiene **seis posiciones normales** si no se requiere reserva distribuida.
- Con cero o una alarma `DISTRIBUTED` elegible participa en las seis posiciones normales.
- Con dos o más distribuidas elegibles se reservan **cinco posiciones normales y una distribuida**; mantener la reserva aunque existan posiciones normales vacías.
- El conjunto distribuido rota independientemente en su posición; el conjunto normal forma otra cola.
- Cuando las alarmas normales elegibles exceden posiciones visibles, las que esperan entran por rotación. No rotar sólo por cadencia sin espera salvo decisión posterior.

**OPEN:** intervalos, fuente de configuración, prioridad de altas/bajas, sincronización de colas, recuperación tras reinicio, codec y shape temporal; no inventar valores por defecto.

## 7. DECISION AGREED: QUEUE_IN_QUEUE futuro para Integrated Operations

Integrated Operations muestra **tres posiciones Mina y tres Planta**. Los componentes elegibles de cada área compiten por sus tres posiciones; cada componente puede contener varias alarmas activas esperando representación.

Dos niveles de rotación:

1. Entre componentes elegibles, para evitar invisibilidad permanente fuera de tres posiciones de su área.
2. Entre alarmas activas del mismo componente, para mostrar las que esperan en su posición.

Fairness, orden determinista de colas, duración visible, interacción entre niveles, refresh y recuperación siguen OPEN. Los vínculos/scope de componente dependen de Tool evidence congelada; no inventar nuevas asociaciones.

## 8. Frontera B.2 / Delivery / Web — CURRENT y posterior

- `AlarmResolutionKey` correlaciona Runtime y Delivery; B.2 conoce Tool kind y visual targets, no planifica carruseles ni geometría.
- Live Delivery futuro sólo podrá exponer occurrences autorizadas por Core/contrato de visibilidad: nunca promover `TRACE_ONLY`, `ECLIPSED` o `CASCADE_SUPPRESSED` por disponibilidad de slots.
- Web consumirá información resuelta, sin leer latest Tool Catalog, recalcular prioridad/routing ni tratar `cause_template` como texto autoritativo final.
- La Web histórica sirve de referencia visual/producto, no autoridad arquitectónica automática.
- `16_ALARM_LIVE_DELIVERY_CONTRACT.md` gobierna el contrato Live separado.

El scheduler de presentación permanece posterior a Materialization, Runtime y Live Delivery. **C4 PLANNED** cambió la dirección deseada del receptor: sólo último CURRENT, sin consumo de FACTS atrasados; este documento no simula que ya ocurrió.

## 9. Editor: implementado, no validado visualmente, OPEN

**CURRENT / COMPONENT TESTS históricos GREEN:** familias/Rules/Messages, tarjetas, input Nueva familia, ayudas/controles dinámicos, Tool→Component→Subcomponent, strict routing, separación `routing_tools`/visual `tools`, estado de autoría y persistencia según suites de componente.

**UNVERIFIED:** host/browser completo posterior al routing y C1, responsive real, Source Blob→Projection Cosmos física y recovery del host tras reinicio en integración externa.

**OPEN / decidir con evidencia, no inferir implementación:**

- generación/instante exacto de `alarm_key` si se cambia política actual;
- restricción sugerida de un único `component_key` en Process: contrato vigente admite varios; no migrar preventivamente;
- guardado/recovery de drafts incompletos y concurrencia Manager sin cambiar reglas genéricas;
- resolver independencia visual/routing frente a sincronización CURRENT;
- evaluator qualificado/hints basados en schema sólo cuando exista productor real.

`is_special_condition` marca la Rule; `reappearance.special_conditions` referencia otras Rules especiales de su familia/grupo. No confundir ambos campos al integrar.

## 9.1 Delta B1d y C1 — Web Tool Catalog

En B1d se observaron dos Tools consolidadas en prueba local y una interfaz funcional de catálogo, **sin aprobación estética final**. La formulación histórica «UI continúa en el host temporal y hay que extraerla» está **SUPERSEDED**: UI/callbacks se extrajeron a `web/tools/catalog-manager` y C1 completó la migración de `catalog`/`discovery-cosmos` a Web. El host temporal compone esa biblioteca y sus providers. **OPEN / SEPARATE:** revisión visual, style/pages y host/browser tras C1, sin tests que congelen CSS visual.

Alarm Configuration permitió observar una Alarm Source local; NO se validó Alarm Source/Projection durable. **OPEN:** modal se cierra sólo después de guardar correctamente, permanece abierto ante validación/error. **OPEN/SEPARATE:** límite de desactivación hasta fin del turno requiere semántica/calendario definidos antes de modificar Domain. Estas observaciones no autorizan cambiar routing ni visual targets.

## 10. Cierre y siguiente foco actual

Routing estricto y suites de componente constituyen un cierre histórico de ese frente; aceptación browser/E2E sigue UNVERIFIED. C1 ha cerrado el ownership Tool Web, no la UX. El siguiente foco exclusivo de este traspaso es **C2: APPLICATION/rutas/Source Key comunes y contenedor Cosmos por contrato de los jobs de alarma**. Las agendas visuales, fin de turno, modal, C3 Qualification, C4 Delivery y Live permanecen en otros incrementos, no deben reabrirse durante C2.

## 11. Conflictos sobrevivientes

- Documentación histórica sobre inexistencia de Cosmos adapter está SUPERSEDED como afirmación del código, pero uso físico Azure y productor operacional NO están probados.
- Formulaciones B.1 antiguas de Special Cascade pueden diferir del tratamiento por prioridad Core actual: no resolver implícitamente al modificar configuración.
- `process_projection_mode` pertenece a cada visual target de una Rule, no a toda su familia.
- Project objetivo Python 3.14.7 vs metadata Command Center `==3.14.2`: OPEN/SEPARATE.
