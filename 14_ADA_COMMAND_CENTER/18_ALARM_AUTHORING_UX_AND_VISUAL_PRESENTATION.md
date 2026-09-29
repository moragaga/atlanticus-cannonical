# ADA Command Center — Alarm Authoring UX and Deferred Visual Presentation

Estado: **CURRENT — componentes editor/strict routing y UX-01/UX-02 implementados; creación/asignación/guardado aceptados manualmente en browser local aislado (2026-09-29). Calidad visual final, recuperación y Live Presentation OPEN.** No confundir el cierre funcional acotado con certificación integral.

## 1. Autoridad y alcance

Histórico: acuerdo inicial de producto del 2026-09-24 y refinamiento routing del 2026-09-26. Owner Tool Web C1 ya migrado. Inspección de este cierre:

```text
Implementation       moragaga/atlanticus:main@2e7500a6b8b4d5bbdad26d807abfa57936db99d5
Decisions            moragaga/atlanticus-decisions:main@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical previo     moragaga/atlanticus-cannonical:main@2e8bbf4780cafc4cea3b18351861aa97a4fb0053
Ownership C1 previo  moragaga/atlanticus@3961385aecd0eb7e373018fc25e509a71dccc409
```

Se conserva el acuerdo visual/routing histórico sin convertirlo en un contrato adicional por conveniencia. El siguiente Starter no autoriza cambiar Source v3, Engine, scheduler, routing ni contratos visuales para facilitar UI.

## 2. CURRENT: configuración y ownership

```text
AlarmConfiguration(rules, messages)
AlarmConfigurationSnapshot(configuration, tool_dependencies)
schema_version = 3
```

Cada Rule posee `AlarmIdentity(family_key, alarm_key)`, color semántico y `visual_targets` con `tool_key`, `component_keys`, `subcomponents` y, para Process, `process_projection_mode`. Cada subcomponent se identifica con `(owner_component_key, subcomponent_key)`. El Tool Catalog confirmado es autoridad de identidad, kind y estructura Tool; una Source Alarm congela el subconjunto exacto de ToolDependencyManifest Cn.

Source/base projection, Stores Local/Cosmos y composición Alarm Configuration existen. El enlace físico Blob/Cosmos E2E está **UNVERIFIED**; el lanzador aislado de prueba no lo ensaya. Materialization READY/BLOCKED sí existe en la implementación anterior; la automatización de Qualification GREEN real sigue separada.

**Owner Tool C1 CURRENT:** `web/tools/catalog` consolida/persiste, `web/tools/discovery-cosmos` inspecciona/confirm, `web/tools/catalog-manager` expone su propia UI/callbacks. Ninguno vuelve a `backend/tools` por estar escrito en Python. La normalización transversal de algunas estructuras aún definidas en `ada-web-tools` permanece fuera del presente hito.

## 3. Familias, prioridad y experiencia — CURRENT/DECIDED

- Familias **derivadas**, no entidad durable duplicada: Rules `identity.family_key` y Messages `scope=FAMILY`. Messages `GLOBAL` se administran aparte y pueden ser referenciados desde familias.
- La interfaz prepara una nueva familia en sesión; se incorpora al documento con su primera Rule o Message. No persistir familias vacías.
- Tarjetas de familia ofrecen Administrar/Eliminar; tarjetas Rule muestran grupo/ranking. Nueva familia usa primitives comunes.
- Reutilizar shell/tokens/estilos/controles de Atlanticus, sin tema propio o CSS duplicado para Alarm. No crear tests que congelen spacing/branding/CSS; validar apariencia manualmente.

**VERIFIED mediante aceptación local reportada por el usuario (2026-09-29):** se pudo crear familias, crear/editar alarmas y mensajes, asignar mensajes a alarmas y guardar. Se usó un script de prueba **externo al repositorio**, que abre el host real con Source/Projection en archivos aislados y **una Tool Process de fixture en File Catalog**, sin emuladores; no equivale al host productivo completo ni a un test de reinicio.

## 4. Strict routing — DECIDED / IMPLEMENTED

```text
PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC -> END
```

Sin mismo nivel, retroceso ni saltos, incluido PROCESS→STRATEGIC. Cada step habilitado avanza un nivel respecto del último habilitado; uno deshabilitado no concede salto. C1/C2 pueden quedar en origen; C3 se limita a origen.

- C1: escalones habilitados inmediatos.
- C2: espera entera positiva por escalón habilitado; B.2 suma esperas en orden y Core programa deadlines relativos al inicio de occurrence (`20 + 20` → minutos 20 y 40).
- C3: sin escalones habilitados; configuración incompatible queda visible para corrección, no se borra silenciosamente.
- Kind proviene de Tool evidence exacta congelada. Domain, B.2 y Web comparten `next_routing_tool_kind`.
- STRATEGIC puede ser destino del routing, pero no tiene visualización Alarm contratada.

## 5. DECISION AGREED: destinos visuales y estrategia futura

Los visual targets indican dónde y sobre qué Component/Subcomponent representar una Rule con su color y, para Process, el modo `GENERIC`/`DISTRIBUTED` editado. La futura estrategia temporal se deriva de Tool kind, no de otro selector Rule:

| Tool kind | Estrategia futura | `process_projection_mode` |
|---|---|---|
| `INTEGRATED_OPERATIONS` | `QUEUE_IN_QUEUE` | `None` |
| `PROCESS` | `CAROUSEL` | `GENERIC` o `DISTRIBUTED` |
| `STRATEGIC` | Sin Alarm visual contratada | No permitido como visual target |

`CAROUSEL`/`QUEUE_IN_QUEUE` **no** son campos Source v3 ni un scheduler actualmente implementado:

```text
visual_targets + color      -> authored contract
B.2 + frozen Tool evidence  -> qualification/resolution
[future] Live Delivery      -> eligibility / temporal planning
Web                         -> geometría y representación concreta
```

**CONFLICT heredado OPEN:** acuerdo visual considera los targets independientes del routing, pero editor Git conserva `synchronize_visual_targets`, que puede derivarlos desde origen y escalones habilitados (excepto STRATEGIC). No atribuir a Starter una decisión implícita sobre la intersección visual/routing; tratarla en incremento independiente si bloquea un caso real.

## 6. DECISION AGREED: CAROUSEL futuro para Process

- Process dispone de **seis posiciones normales** si no se requiere reserva distribuida.
- Con cero o una alarma `DISTRIBUTED` elegible, utiliza las seis posiciones normales.
- Con dos o más distribuidas elegibles se reservan **cinco posiciones normales y una distribuida**, incluso si hay posiciones normales vacías.
- El conjunto distribuido rota por su posición reservada; las alarmas normales forman otra cola.
- Cuando las normales elegibles excedan plazas visibles, las restantes entran por rotación; no rotar sólo por cadencia sin espera salvo decisión nueva.

**OPEN:** intervalos, fuente de configuración, prioridad de entradas/salidas, sincronización de colas, recuperación tras reinicio, codec y shape temporal. No inventar valores por defecto.

## 7. DECISION AGREED: QUEUE_IN_QUEUE futuro para Integrated Operations

Integrated Operations muestra **tres posiciones Mina y tres Planta**. Los componentes elegibles de cada área compiten por sus tres posiciones; un Component puede contener varias alarmas activas esperando representación.

Dos niveles independientes de rotación:

1. Entre Components elegibles, para evitar invisibilidad permanente de los que esperan fuera de las tres posiciones del área.
2. Entre alarmas activas del mismo Component, para mostrar las que esperan dentro de la posición del Component.

Fairness, orden determinista, duración visible, sincronización entre niveles, refresh/recovery y vínculos elegibles continúan **OPEN**. No inventar asociación Tool/Component fuera de la evidence congelada.

## 8. B.2 / Engine / Delivery / Web — fronteras CURRENT y futuras

- `AlarmResolutionKey` correlaciona configuraciones Runtime/Delivery exactas. B.2 conoce Tool kind y visual targets, pero no planifica carruseles ni geometría.
- Engine conserva WAL/EFFECTIVE y publica CURRENT v1 completo; FACTS v2 inmutables permanecen en Runtime. **C4 ya sustituyó** el receptor Delivery CURRENT+FACTS por recepción exclusiva del último CURRENT con validación de pin/EFFECTIVE/READY; el receptor **no** construye Live por ese hecho.
- El futuro Live Delivery sólo expone occurrences autorizadas por prioridad/visibilidad del Core. `TRACE_ONLY`, `ECLIPSED` y `CASCADE_SUPPRESSED` no ganan visibilidad por huecos de presentación; deactivated y managed no implican inactividad física.
- Web consume estado resuelto, no relee latest Tool Catalog, no recalcula prioridad/routing ni interpreta `cause_template` como causa final.
- El Web histórico orienta lenguaje visual de producto, no transfiere autoridad arquitectónica por analogía. Live y Management/History siguen separados; ver `16_ALARM_LIVE_DELIVERY_CONTRACT.md`, que requiere actualización de estado C4 desde el corte canónico antiguo.

## 9. Editor: aceptación funcional básica CLOSED; calidad completa OPEN

**CURRENT / IMPLEMENTED:** familias/Rules/Messages, controles de Tool→Component→Subcomponent, strict routing, separación de opciones de Tool routing/visual, persistencia local de workspace y controles del Manager. Tests de componente anteriores y posteriores aportan cobertura; no se deben confundir con aceptación global de browser.

**UX-01 — CLOSED:** Save Draft de modal cierra en caso exitoso; ante error debe permanecer abierto. Test suite Web reportada tras este incremento: **122 PASS**.

**UX-02 — CLOSED en configuración:** Rule default y Message override presentan selector de **1–11 horas** más `Fin del turno` (`END_OF_SHIFT`). Domain y Materialization transportan el límite estático; Source sigue v3. El usuario informó en tres suites UX-02 **59+64+123=246 PASS**, `git diff --check` limpio y commits subidos. La prueba local permitió operar el editor real con fixture de Tool; no se informó selección manual/round-trip completo del valor `END_OF_SHIFT` ni acción operacional que calcule `effective_until`.

**VERIFIED por el usuario:** apertura del host aislado, creación de familias, alarmas y mensajes, asignación y guardado. **UNVERIFIED:** reinicio y recovery, conexión física Source Blob/Projection Cosmos, CI/Docker/Azure, aceptación responsive y error matrix exhaustiva de UX-01/UX-02.

**OPEN como findings observados por el usuario:** valores de campos que desaparecen, validaciones que se pierden y avisos `success`/`warning`/`danger` persistentes. No se diagnosticó su causa ni se autoriza dar por resuelta la calidad productiva. La pérdida de valores debe investigarse antes de promover el editor a producción.

**OPEN de autoría heredados:** instante/generación de `alarm_key` sólo si cambia la política existente; sugerencia de limitar Process a un solo `component_key` **no está adoptada** (contrato actual admite varios); guardado/recovery/concurrencia de drafts incompletos sin romper Manager; separación visual/routing; schema/hints de evaluator sólo con productor autorizado. `is_special_condition` marca Rule; `reappearance.special_conditions` referencia Rules especiales de su family/group: son conceptos distintos.

## 9.1. Tool Catalog Web y entorno de prueba

C1 está **CLOSED**: la formulación antigua «Tool Catalog UI vive en host temporal y debe extraerse» es **SUPERSEDED**; `web/tools/catalog-manager` posee UI/callbacks. El host sólo compone librerías y providers. El Tool Catalog histórico B1d se confirmó con dos Sources/Projections bajo emuladores; no acreditar Source/Projection Alarm durable con ello.

El `__main__` temporal con `ADA_MANAGER_PERSISTENCE_PROVIDER=local` usa archivos para Alarm Source/Projection **pero sigue usando Storage/Azurite** para Tool Catalog. El lanzador externo sin emuladores utiliza `TestOnlyFileCatalog` local, `tool_catalog_manager=None` y una Tool Process ficticia, **únicamente para validar UX**. No integrarlo como código productivo, no inventar provider local nativo por analogía y no distribuirlo como Starter.

## 9.2. Fin del turno: frontera estática frente a operacional

El acuerdo de este hito distingue opción authored `END_OF_SHIFT` de `effective_until` UTC. Web operacional es quien conoce/obtiene la hora real de término del proceso/turno; el Core recibe el instante y **no** un algoritmo de calendario. **OPEN:** proveedor real, zona, comprobación de límites y acción de Management Capture/Web operacional; no se implementaron en el editor de configuración.

**CONFLICT con Decisions DESIGN FROZEN B.1 sección 11:** aún declara entero `1..12` y `shift_end` pendiente, mientras que Git implementa `1..11 | END_OF_SHIFT` bajo campo `max_duration_hours`, sin versionar Source v3. No reinterpretar `12` como `END_OF_SHIFT` ni añadir legacy. Registrar decisión formal/versionado según evidencia de consumidores.

## 10. Siguiente foco propuesto: Starter genérico, sin abrir otra UX

**PLANNED / único foco:** debatir y diseñar la composición **propia** de Command Center Web, con runtime existente y página de inicio mínima, como superficie de verificación de un recorrido real capaz de ejecutar alarmas. Excluir Navigation, Users, Profiles, scheduler visual, dashboard complejo e History/Analytics en este primer incremento. Contratos/backend antes de consumidores/frontend; verificar dónde termina CURRENT-only y qué falta de Live si se quiere ver alarmas operacionalmente en la home.

## 11. Conflictos sobrevivientes

- Los textos anteriores que declaran falta de Cosmos adapter/Materialization ejecutable están SUPERSEDED como descripción del código, no como qualification física Azure.
- B.1 frozen Special Cascade puede contradecir suppression uniforme por ranking del Core; tratar como conflicto declarado, no refactor implícito.
- Message inactivo válido/no seleccionable para nuevas gestiones requiere conservar las discrepancias históricas de B.1/B.2 explícitas.
- `process_projection_mode` pertenece a cada visual target de Rule, nunca a toda la familia.
- Metadata de varios paquetes Command Center fija `requires-python==3.14.2`; baseline Project 3.14.7 y prompt de shell local 3.14.7; el intérprete elegido por `uv run` no se verificó. Compatibilidad de empaquetado/lock/distribución separada OPEN.
- Canonical remoto anterior a este reemplazo también describe C4 como PLANNED; no sobrescribir los reemplazos C4 que otro chat hubiera preparado sin fusionar su detalle.
