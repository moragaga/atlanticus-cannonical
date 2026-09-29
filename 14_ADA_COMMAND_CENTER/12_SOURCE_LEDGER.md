# ADA Command Center — Source Ledger

Estado: **AUDIT LEDGER — genealogía anterior conservada; delta Web/Tool Catalog B1d añadido el 2026-09-29**. La sección histórica «Current implementation» conserva su fecha y no identifica el HEAD actual.

## Current implementation

```text
moragaga/atlanticus@880cb692054c2cd78cdc29cf62ab7b16bbd2c3d6
```

Relevant closure commits:

```text
9b9600ae96c9153cf70d0fb401905963b8583c2f
-> Command Center domain/tools + ToolDependencyManifest

d2a5e14822d3711e64668b8e70cfa15d7ddae2f0
-> AlarmConfigurationSnapshot v3
-> Tool manifest persistence
-> workspace Tool revision pin
-> validation/publication drift guard
-> local runtime integration
```

Commits after `d2a5e14822d3711e64668b8e70cfa15d7ddae2f0` up to `880cb692054c2cd78cdc29cf62ab7b16bbd2c3d6` belong to an unrelated operational-data front.

## Current canonical base inspected

```text
moragaga/atlanticus-cannonical@148b178df74ee3083681140f3bb7997a02435b80
```

## Historical decisions

```text
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

## Qualification observed

```text
domain/tools
8 passed

domain/alarms
50 passed

web/alarms/configuration
35 passed

configuration-manager
11 passed
```

Lint and format gates GREEN.

## Contract changes

### Added

```text
scopes/ada-command-center/domain/tools
ToolDependencyEntry
ToolDependencyManifest
```

### Alarm Source

Previous:

```text
AlarmConfigurationSnapshot
    configuration
    confirmed_tool_catalog_revision
```

CURRENT:

```text
AlarmConfigurationSnapshot
    configuration
    tool_dependencies
```

`confirmed_tool_catalog_revision` is derived.

```text
v2 -> SUPERSEDED
v3 -> CURRENT
```

No legacy v2 decoder.

### Workspace

Added:

```text
_confirmed_tool_catalog_revision
```

Manager generic unchanged.

### Tool reader

One Tool snapshot read now produces authoring references + dependency evidence.

## Nuevo corte B1d — implementación y qualification delimitadas (2026-09-29)

```text
atlanticus:main implementado B1d         a518ff98c6303220e24ae3c645d3982e657fd22e
atlanticus:main inspeccionado             caced5d7711cf059d36ec61aecc9b3e9629bd41f
atlanticus-decisions:main                50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
atlanticus-cannonical:main docs leídos ec16bd2ccf0ae06065b8ee1d3a231ef4d2cbac57
atlanticus-cannonical:main al entregar a5bb42157ee7a5dd2fd64ccc43fa4519628ce25c
```

**CURRENT en Git:** `backend/tools/discovery-cosmos` descubre/inspecciona/consolida; `backend/tools/catalog` persiste CURRENT; `web/application/ada-command-center-configuration-manager/catalog_manager.py` implementa la UI, montada como `ManagerEntry`. `web/alarms/configuration` continúa capability propia. El host documenta `ATLANTICUS_ENVIRONMENT` y `ADA_MANAGER_PERSISTENCE_PROVIDER`, pero `.env` de prueba se mantiene excluido.

**Qualification local informada:** 38 backend tests, 31 Manager tests, 6 tests de qualification y Ruff/format PASS en el último parche; dos Sources/Projections de Tool generadas con APIs existentes, descubrimiento/confirmación y lectura cruzada de catálogo en Azurite. Revisión `6a26feedc3cf7cee4ebcf5a93ad59314180635875ab25423bb576a052e517243`. Alarm Source `local` release `1d76643e80f849cc931702689aec45a6`; `verify-alarm` durable informó ausencia de Source/Projection. El paquete `qualification/tool-catalog-b1d` se observó localmente; no se encontró en el árbol remoto inspeccionado. No declarar CI ni Azure productivo.

**Nueva decisión de Project / NO IMPLEMENTADA:** extraer Tool Catalog como biblioteca Web de Command Center y crear después un Starter distribuible que componga Tool Catalog y Alarm Configuration. Auditar residuos, `.env.detail`, rutas y el alcance de los archivos de qualification; no eliminar evidencia sin inventario. La revisión visual de Tool Catalog y los dos ajustes Web de alarmas son OPEN separados.

## Historical conflicts

`atlanticus-decisions` remains useful history but is stale in:
- SharePoint physical authority;
- re-resolution against later Tool revision without Alarm republish;
- consolidated Tool output to Command Center Cosmos.
