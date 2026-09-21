# ADA Command Center — Source Ledger

Estado: **AUDIT LEDGER / UPDATED 2026-09-21**

## `atlanticus:main`

Checkpoint CURRENT de cierre:

```text
1c67212b21ef2241bcb59173ccb8e9cd237a0219
```

Parent inmediato:

```text
07eeb8d4ecc3f1e9d9a84ab1059eaad2fd5f78ce
```

Tree:

```text
ae432b5aa55183ca9f11b35ac37f4fa3859c9e78
```

### Commits del hito

```text
bc8eafc21a65e3f9aff044c232e2562cd490c49f
→ baseline auditado antes de materializar Command Center Web

8e5f7312eb7522a2345b1d225faa80c6eac6ec41
→ Alarm Configuration contract + Source/Release

07eeb8d4ecc3f1e9d9a84ab1059eaad2fd5f78ce
→ Alarm Configuration base Projection

1c67212b21ef2241bcb59173ccb8e9cd237a0219
→ Manager/workspace/workflows + capability-local Web surface
```

Inspeccionado/relevante:

- `scopes/ada-command-center/backend/alarms/core`;
- `scopes/ada-command-center/backend/alarms/persistence`;
- `scopes/ada-command-center/backend/processes/alarms-runtime`;
- `scopes/ada-command-center/web/alarms/configuration`;
- `scopes/ada/web/tools/core`;
- `scopes/ada/web/tools/configuration`;
- `scopes/ada/web/tools/projection-cosmos`;
- `scopes/ada/web/application/ada-configuration-manager`;
- `web/capabilities/manager`;
- `web/capabilities/projection`;
- `web/capabilities/source`.

## Evidencia de tests del hito

Ejecutado en checkout real:

```text
Alarm Configuration initial contract     17 passed
+ base Projection                         20 passed
+ Manager integration                     27 passed
ruff check .                              All checks passed
ruff format --check .                     clean before published checkpoint
```

## Alarm Configuration CURRENT

Existe físicamente:

```text
scopes/ada-command-center/web/alarms/configuration
```

Implementa:

- `AlarmConfiguration` aggregate Rules + Messages;
- full-revision intrinsic validation;
- Source codec/service;
- exact Source/Release round-trip;
- base `SourceProjectionService[AlarmConfiguration]`;
- Manager source workflow;
- Manager draft validation workflow;
- Manager workspace binding;
- reusable `ManagerModule` composition;
- capability-local Web surface/document editor;
- history preview;
- commented pedagogical mirror.

No modificó `ada-command-center/backend/alarms/core` durante este hito.

## Tool Configuration CURRENT relevante

ADA Tool Configuration usa `tool_key` como identidad contractual y exige consistencia de esa key en
consumption, operational participation y structure.

`ToolConfigurationKind` CURRENT:

```text
PROCESS
INTEGRATED_OPERATIONS
STRATEGIC
```

`ToolStructure` ya expresa Components, Subcomponents y relaciones necesarias para resolución.

El siguiente Tool Catalog de Command Center debe partir de estos contratos reales, no inventar un
modelo de authoring paralelo.

La unicidad global entre Tools continúa como invariante de diseño; el punto exacto que genera/enforce
esa unicidad permanece UNVERIFIED/OPEN.

## Generic Manager CURRENT

La capability genérica Manager provee shell/workflow/composición Source/Projection reutilizable.

Alarm Configuration ya la consume mediante `ManagerModule`.

No se creó una copia del Manager ADA.

El montaje dentro de la futura aplicación/shell Command Center permanece abierto.

## Source / Projection CURRENT

SourceStore, Release y exact ProjectionTarget están CURRENT.

La base Alarm Configuration Projection no tiene dependencies externas. B.2 será una resolución
posterior que combine snapshots/provenance y no debe deformar la Projection base para imponer orden.

## Command Center Tool Catalog

No existe implementación en `atlanticus:main` al checkpoint CURRENT.

Dirección congelada:

```text
external ADA Tool projections
→ Command Center consolidator/reconciler
→ durable read-only Tool Catalog revision/LKG
→ B.2 consumer
```

No es ADA Tool Configuration authoring y no debe crear un segundo Cosmos sólo para duplicar la
Tool topology.

## B.2

Búsqueda/inspección del checkpoint CURRENT no encontró `ResolvedAlarmConfiguration` ni una
implementación física B.2.

Runtime CURRENT continúa usando:

```text
alarm_configuration_revision
tool_registry_revision
```

como strings históricos que deberán reconciliarse posteriormente con provenance CURRENT.

## `atlanticus-cannonical`

Checkpoint inspeccionado antes de este reemplazo:

```text
f04ee728b157a4f64a3c0c59622d5d6702f4cd87
```

Ese canonical ya congelaba:

- intrinsic validity separada de external resolution/readiness;
- parameters genéricos `str | float | bool`;
- Tool Catalog read-only reconciliado y durable en Blob;
- múltiples Cosmos mediante conexiones nombradas;
- misma Alarm Source revision re-resoluble contra Tool Catalog posterior.

Quedó desactualizado respecto de implementación al seguir indicando que `scopes/ada-command-center/web`
y Alarm Configuration estaban pendientes.

## `atlanticus-decisions`

Uso: **HISTORICAL ONLY**.

Checkpoint observado:

```text
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

### B.1

`alarm_decisions/R3.6M-006B.1-alarm-definition-contract-inventory-DESIGN-FROZEN.md`

Preserva semántica útil del contrato AlarmDefinition.

### B.2

Preserva decisiones históricas sobre:

- Live vs Management Projection;
- publication/materialization;
- Runtime/Delivery desde una resolución común;
- LKG;
- distinction INVALID/REMOVED.

Refinamientos CURRENT frente a historia:

- bindings históricos SharePoint no son autoridad en dominios migrados;
- Tool/evaluator no son requisito para persistir una Source revision intrínsecamente válida;
- referencia Message inactiva es intrínsecamente válida en CURRENT implementation;
- external unresolved no equivale a invalid;
- Runtime y Delivery pueden tener readiness diferente.

## Evidencia Web histórica

Existen prototipos/implementaciones históricas de dashboards de alarmas.

Son REFERENCIA, no autoridad de la Web nueva.

No portar automáticamente:

- CSS;
- geometría;
- polling;
- Redis assumptions;
- contratos snapshot antiguos.
