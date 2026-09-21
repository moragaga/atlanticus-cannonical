# ADA Command Center — Source Ledger

Estado: **AUDIT LEDGER / UPDATED 2026-09-21**

## `atlanticus:main`

Checkpoint auditado para este refinamiento:

```text
bc8eafc21a65e3f9aff044c232e2562cd490c49f
```

Inspeccionado/relevante:

- `scopes/ada-command-center/backend/alarms/core`;
- `scopes/ada-command-center/backend/alarms/persistence`;
- `scopes/ada-command-center/backend/processes/alarms-runtime`;
- `scopes/ada/web/tools/configuration`;
- `scopes/ada/web/tools/projection-cosmos`;
- `web/capabilities/manager`;
- `web/capabilities/projection`;
- `web/capabilities/source`;
- `connectivity/cosmos`;
- `web/capabilities/storage/topology`;
- `web/capabilities/storage/cosmos`.

En este checkpoint no existe todavía una superficie `scopes/ada-command-center/web` materializada.

## Evidencia CURRENT relevante

### Tool Configuration

`ToolConfiguration` usa `tool_key` como identidad contractual y exige consistencia de esa key en consumption, operational participation y structure.

La unicidad global entre Tools es una invariante de diseño confirmada para Command Center; el punto exacto que genera/enforce esa unicidad no fue verificado en este audit.

### Generic Manager

La capability genérica Manager ya provee shell/workflow/composición Source/Projection reutilizable.

Command Center debe reutilizarla para su superficie administrativa y no copiar el Manager ADA.

### Source / Projection

SourceStore, Release y exact ProjectionTarget están CURRENT.

La documentación previa que condicionaba Alarm Configuration a que SourceStore quedara congelado quedó obsoleta.

### Cosmos / named resources

`CosmosSettings` modela una conexión Cosmos resuelta.

Web Storage Topology usa `connection_ref` y el bridge Cosmos acepta provisioners agrupados por conexión.

Esto soporta composición con múltiples conexiones nombradas sin introducir un Cosmos global.

## `atlanticus-decisions`

Uso: **HISTORICAL ONLY**.

### B.1

`alarm_decisions/R3.6M-006B.1-alarm-definition-contract-inventory-DESIGN-FROZEN.md`

Preserva semántica útil del contrato AlarmDefinition.

### B.2

Preserva decisiones históricas sobre:

- Live vs Management Projection;
- publication/materialization;
- persistence gate;
- latest valid;
- Runtime/Delivery resolution.

Las implementaciones físicas históricas SharePoint/Cosmos no son autoridad cuando contradicen el baseline CURRENT Blob/Source/Projection.

## Refinamientos de este hito

SUPERSEDED:

- Tool disponible como requisito para persistir una Alarm Configuration revision;
- Component/Subcomponent externo resuelto como requisito de intrinsic validity;
- evaluator-specific parameter schema como responsabilidad de Alarm Configuration UI;
- duplicar el Tool Catalog de Command Center en un Cosmos propio sólo para consumo de configuración.

CURRENT / FROZEN DIRECTION:

- intrinsic validity separada de external resolution/readiness;
- parameters genéricos `str | float | bool`;
- Tool Catalog read-only reconciliado y durable en Blob;
- múltiples Cosmos mediante conexiones nombradas;
- misma Alarm Source revision puede re-resolverse contra una Tool Catalog revision posterior.

## Evidencia Web histórica

Existen prototipos/implementaciones históricas de dashboards de alarmas.

Son REFERENCIA, no autoridad de la Web nueva.

No portar automáticamente:

- CSS;
- geometría;
- polling;
- Redis assumptions;
- contratos snapshot antiguos.
