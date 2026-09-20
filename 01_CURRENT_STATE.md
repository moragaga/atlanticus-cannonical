# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

## Autoridad

Implementación publicada CURRENT:

```text
moragaga/atlanticus@d71e94d12fa31a986b3ecc0262fbbb6ef2e4a3dd
```

Parent inmediato:

```text
107c7570061e0d31828b1d3e9b9fc6336a698809
```

Tree:

```text
41c299861d14a9691cbd3461dbca8bb466dfc156
```

Canonical inspeccionado para este cierre:

```text
moragaga/atlanticus-cannonical@a3e77aafe5e97bd4e2e10a9d9b24be9ff0486471
```

Git permanece SOLO LECTURA para el asistente.

## Estado resumido

```text
KPI-REGISTRY-CAPABILITY-CUTOVER                 CLOSED / VERIFIED / CURRENT
KPI-DEFINITION-CAPABILITY-CUTOVER               CLOSED / VERIFIED / CURRENT
KPI-MANAGER-REGISTRY-WIRING                     CLOSED / VERIFIED / CURRENT
KPI-MANAGER-DEFINITION-WIRING                   CLOSED / VERIFIED / CURRENT

KPI-RUNTIME-REPROCESS-CURRENT                   PLANNED / NEXT
KPI-DELIVERY-REGISTRY-CONSUMPTION               PLANNED
KPI-TIMESERIES-REGISTRY-CONSUMPTION             PLANNED
KPI-HISTORIAN-REPROCESS-CURRENT                 PLANNED

ADA-GENERIC-COLLECTOR-CLOSURE                   BLOCKED / AFTER KPI BACKEND FLOW

KPI-INSPECTION-DEFINITION-PROVIDER-REALIGNMENT  OPEN / SEPARATE
PYTHON-METADATA-ALIGNMENT                       OPEN / SEPARATE
```

## KPI Registry CURRENT

```text
scopes/ada/web/kpis/registry/
├── core
├── configuration
├── projection-local
└── projection-cosmos
```

Domain:

```text
KpiRegistry
KpiRegistryBinding
```

Source:

```text
SourceKey('kpis')
```

Projection:

```text
ProjectionRecord[KpiRegistry]
```

Dependency:

```text
Tool ProjectionTarget
→ KPI Registry ProjectionTarget
```

Cosmos:

```text
logical_id = ada.kpis.registry.projection
physical   = ada-kpi-registry-projection
partition  = /partition_key
TTL        = None
document_type = ada_kpi_registry_projection_record
```

## KPI Definition CURRENT

```text
scopes/ada/web/kpis/definition/
├── core
├── configuration
├── projection-local
└── projection-cosmos
```

Domain:

```text
KpiDefinition
KpiDefinitionConfiguration
KpiDefinitionCatalog
```

Source:

```text
SourceKey('kpi-definitions')
resource = kpis/definition.json.gz
```

Projection:

```text
ProjectionRecord[KpiDefinitionCatalog]
```

Dependency:

```text
KPI Registry ProjectionTarget
→ KPI Definition ProjectionTarget
```

Cosmos:

```text
logical_id = ada.kpis.definition.projection
physical   = ada-kpi-definition-projection
partition  = /partition_key
TTL        = None
document_type = ada_kpi_definition_projection_record
```

## Local Manager CURRENT

Registry y Definition usan projection stores locales durables.

No dependen de `InProcessProjectionStore` para esas dos projections.

La UI fue preservada visualmente durante ambos cutovers.

## Backend CURRENT

### KPI Runtime

CURRENT todavía corta:

```text
observed == committed
→ reason=up_to_date
→ skip
```

No existe todavía `REPROCESS_CURRENT`.

### Historian

CURRENT todavía corta:

```text
historian authority == KPI committed
→ SKIPPED_CURRENT
```

No existe todavía `REPROCESS_CURRENT`.

### Delivery / Timeseries

CURRENT todavía consume:

```text
document_type = ada_kpi_configuration_projection
payload = configuration.bindings
binding identity = key
```

Eso entra en conflicto con KPI Registry CURRENT:

```text
document_type = ada_kpi_registry_projection_record
payload = payload.bindings
binding identity = kpi_key
```

El consumer backend debe migrar sin dual reader ni legacy compatibility.

## Qualification observada

Registry cutover:

```text
108 tests passed
```

Definition cutover:

```text
76 tests passed
```

Además:

```text
git diff --check
PASS observado

Registry UI assets
byte-identical

Definition UI assets
byte-identical
```

No declarar Ruff remoto/full workspace/CI como PASS.

## Conflicto separado CURRENT

KPI Inspection Definition provider continúa referenciando un contrato histórico de Definition:

```text
ada-web-kpi-definition==0.1.0
KpiDefinitionProjectionRepository
```

No fue parte de este cierre.

## Siguiente foco único

```text
KPI-RUNTIME-REPROCESS-CURRENT
PLANNED / NEXT
```
