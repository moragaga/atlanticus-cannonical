# ADA Command Center — Alarm Configuration Authoring Model

Estado: **CURRENT — Source Snapshot v3 y editor guiado; UX-01/UX-02 CLOSED por código, suites de componente y prueba funcional básica local (2026-09-29); recuperación/E2E y defectos de interacción OPEN**.

## 1. Autoridad y alcance de esta actualización

- Implementación auditada: `moragaga/atlanticus:main@2e7500a6b8b4d5bbdad26d807abfa57936db99d5` (incluye UX-02 sobre `128d8a9339f1c7ecd897628b02274a898073cd26`).
- Decisions inspeccionadas: `moragaga/atlanticus-decisions:main@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`. **CONFLICT:** B.1 DESIGN FROZEN, sección 11, conserva `max_duration_hours: int|None` con rango `1..12`, mientras que Git ya acepta `1..11 | END_OF_SHIFT`.
- Canonical remoto anterior a reemplazo: `moragaga/atlanticus-cannonical:main@2e8bbf4780cafc4cea3b18351861aa97a4fb0053`.
- Los tests y la aceptación en navegador se acreditan por salidas/declaraciones del usuario. No son CI, Docker, Azure ni aprobación de cada detalle visual.

## 2. Agregado editable y versión durable — CURRENT

```text
AlarmConfiguration
  rules
  messages

AlarmConfigurationSnapshot
  configuration: AlarmConfiguration
  tool_dependencies: ToolDependencyManifest
  schema_version: 3
```

La familia se **deriva** de `AlarmIdentity.family_key` y de Messages `scope=FAMILY`, sin entidad Family durable. `alarm_key` es estable; `rule_name`, `display_name`, título y causa tienen responsabilidades propias. Una familia creada en la interfaz queda pendiente hasta incorporar su primera Rule o Message: no persistir familias vacías.

El manifest Tool se congela con la revisión Cn en cada Source Rn. Source v2 está SUPERSEDED y no existe decoder legacy. Conservar referencias exactas de Rules activas e inactivas, origen, escalones habilitados/deshabilitados, visual targets y estructura Tool para reconstruir sin latest.

## 3. Workspace, Save, Validate, Publish — CURRENT

El editor trabaja con `AlarmConfiguration` y el sidecar `_confirmed_tool_catalog_revision`, que **no** pertenece al agregado.

```text
Save Draft -> consulta Confirmed Tool Catalog y fija Cn en workspace
Validate   -> valida configuración, Cn actual y referencias Tool
Verify     -> verifica concurrencia Source mediante Manager
Publish    -> revalida Cn (drift guard) y congela ToolDependencyManifest(Cn)
```

Una Source Rold/Cold publicada conserva sus dependencias inmutables; no se reinterpreta por cambios posteriores del catálogo. `VALID_AT_SAVE != READY != EFFECTIVE`. Un borrador incompleto no debe publicarse silenciosamente.

**UX-01 — CLOSED a nivel de implementación/tests:** el guardado de borrador dentro del modal cierra el editor únicamente después del éxito; un fallo no debe cerrarlo ni simular guardado. Existían 122 PASS de suite Web reportados tras UX-01; después de UX-02 la suite Web reportó **123 PASS**. No se acreditó una matriz exhaustiva de errores visuales de modal en navegador.

**Prueba manual básica — VERIFIED/CLOSED:** usuario levantó host real con un lanzador externo de prueba, creó familias, Rules y Messages, asignó Messages a alarmas y guardó. **No** implica recovery tras reinicio, persistencia Blob/Cosmos ni ausencia de defectos adicionales de interacción.

## 4. Routing FROZEN; presentación visual OPEN

```text
PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC -> END
```

No saltar niveles, repetir tipo ni retroceder. C1/C2 pueden permanecer en origen; C3 sólo origen. C1 exige routing habilitado inmediato y C2 espera positiva por escalón; B.2 procesa offsets correspondientes. El editor reutiliza `next_routing_tool_kind`. Configuraciones incompatibles quedan visibles para corrección, sin borrado automático ni reclasificación oculta.

`routing_tools` admite STRATEGIC; el catálogo `tools` de visualización incluye PROCESS e INTEGRATED_OPERATIONS. Cada Rule tiene visual targets con Tool, `component_keys`, subcomponentes `(owner_component_key, subcomponent_key)` y `process_projection_mode` sólo para Process. Las estrategias futuras `QUEUE_IN_QUEUE` (Integrated Operations) y `CAROUSEL` (Process) **no** son campos Source v3 ni scheduler implementado.

**CONFLICT OPEN heredado:** el acuerdo conceptual mantiene independencia de visual targets frente a routing, pero el editor actual puede ejecutar `synchronize_visual_targets` desde origen/escalones habilitados. No cambiarlo dentro de un Starter Web ni asumir resuelto. Detalle en `18_ALARM_AUTHORING_UX_AND_VISUAL_PRESENTATION.md`.

## 5. Parámetros de evaluator — FROZEN

Cada Rule configura `evaluator_key` y `parameters: Mapping[str,str|float|bool]`, sin `None`, listas, diccionarios anidados ni expresiones ejecutables. Los parámetros de negocio son opcionales y pertenecen al evaluador autorizado: Web no impone un `limit`/`factor` universal ni deduce tablas o fuentes de datos. La demo B2c.5d con `limit=80.0` no constituye evaluador de producción.

`(family_key,evaluator_key)` resuelve la implementación; `alarm_key` identifica la Rule. `EvidenceSnapshot` es resultado del backend evaluator, no una licencia para que Web controle WAL/lifecycle.

## 6. UX-02 — política de desactivación de configuración

**CURRENT / CLOSED para editor, Domain, codec y Materialization:** tanto `default_deactivation.max_duration_hours` de Rule como `deactivation_override.max_duration_hours` de Message ofrecen **once intervalos enteros `1..11` y `Fin del turno`**, representado explícitamente por `END_OF_SHIFT`. No se transforma la alternativa en doce horas artificiales.

Implementación observada en `domain/alarms/definition.py`:

```text
DEACTIVATION_MAX_HOURS = 11
END_OF_SHIFT = 'END_OF_SHIFT'
DeactivationLimit = int | Literal['END_OF_SHIFT'] | None
```

El campo serializado **conserva el nombre** `max_duration_hours` y Source **conserva schema v3**. `enabled=false` exige máximo `None` y `approval_required=false`; `enabled=true` requiere `1..11` o `END_OF_SHIFT`. Un Message `deactivation_override=None` hereda el default de Rule; si el override existe, sustituye **COMPLETAMENTE** los tres miembros, no mezcla máximos/aprobación.

**VERIFIED por tests locales proporcionados por el usuario:** Domain **59 PASS**, Materialization **64 PASS**, Configuration Web **123 PASS** = **246 PASS**; `git diff --check` limpio, 20 archivos aplicados y commit Git verificado `2e7500a...`.

**OPEN CONTRACTUAL:** Decisions B.1 frozen todavía exige rango numérico `1..12`, afirma máximo de turno de doce horas y deja `shift_end` fuera de B.1. Cambiar mismo campo a `str|int` sin cambiar schema v3 puede afectar datos/clientes anteriores; compatibilidad concreta **UNVERIFIED**. No inventar decoder legacy, migración ni nueva versión sin decisión explícita. Tampoco interpretar `END_OF_SHIFT` como un plazo operacional resuelto: Web operacional deberá aportar la hora real del proceso/turno y transmitir `effective_until` UTC; zona, calendario, validación y fuente son **OPEN**.

## 7. Tool Catalog y pruebas locales

Ownership CURRENT: `web/tools/catalog` mantiene Tool Catalog confirmado en Blob, `web/tools/discovery-cosmos` inspecciona conexiones nombradas y `web/tools/catalog-manager` ofrece UI reusable; host temporal compone. `backend/tools` está SUPERSEDED. STRATEGIC puede ser destino routing, no visual target contratado.

B1d comprobó históricamente catálogo confirmado y Alarm Source **local**, no alarm Source Blob/Projection Cosmos durable. El lanzador externo de prueba 2026-09-29 utiliza deliberadamente `TestOnlyFileCatalog` con **una Tool Process ficticia** y Source/Projection locales, sin Azurite/Cosmos; este mecanismo de qualification visual **no** es proveedor productivo ni Starter.

## 8. Findings y frontera siguiente

**OPEN UX, no bloqueantes para cerrar estas operaciones básicas:** algunos valores de campos desaparecen, ciertas validaciones dejan de verse y los avisos `success`/`warning`/`danger` permanecen pegados. Se desconoce causa y extensión. La pérdida de valores podría afectar integridad del borrador: no declarar aceptación productiva del editor. Recuperación tras reinicio, aceptación responsive y ejecución real siguen **UNVERIFIED**.

**PLANNED / siguiente foco separado:** auditar y diseñar el Starter **genérico propio de Command Center** con runtime real y home mínima; sin Navigation, Users ni Profiles, y sin transformar el fixture local en implementación. No mezclar refinamientos UX con esta frontera, salvo finding bloqueante demostrado.
