# Atlanticus — Roadmap

Estado: **CURRENT ROADMAP — SOURCE CONVERGENCE NEXT**

## CLOSED / CURRENT

```text
WEB-DISTRIBUTION-SHARED-ENGINE-CLEANUP          CLOSED
DUAL-APP-ENV-DETAIL-CONTRACT                    CLOSED
COMMAND-CENTER-DURABLE-RUNTIME-COMPOSITION      CLOSED
ATLANTICUS-WEB-MASTER-PROJECTION                CLOSED
DUAL-APP-MASTER-PROJECTION-CONVERGENCE          CLOSED
```

## NEXT único

```text
SOURCE-NAMESPACE-AND-COMPOSITION-CONVERGENCE
PLANNED / NEXT
```

Objetivo:

```text
preserve SourceStore Core
preserve Local/Blob provider semantics
inspect current ADA + Command Center Source composition
remove Command Center dependency on ADA-owned namespace contract
define reusable namespace/composition boundary
keep product-specific naming in product composition
```

No mezclar tooling reorganization, KPI/Collector/UI, Entra o Python migration.

## AFTER NEXT

```text
DUAL-APP-DURABLE-RUNTIME-SMOKE
PLANNED
```

Levantar desde source:

```text
ADA Generic
ADA Command Center Generic
```

con:

```text
ATLANTICUS_ENVIRONMENT=local
persistence=durable
```

y conexiones configuradas a los destinos elegidos. Emulator/Azure no cambian el contrato.

Validar:

```text
resource availability
application startup
Master Projection material provisioning/read
projection planning/apply where applicable
restart/readback as needed
```

## THEN — ADA

```text
ADA KPI/data
→ KPI Delivery Latest/Timeseries
→ Collector
→ browser stores
→ UI
```

## DEFERRED — tooling topology normalization

```text
scopes/operational-data/tooling
scopes/ada/tooling/distribution/backend
scopes/ada-command-center/tooling/distribution/backend
root tooling as cross-scope orchestrator
```

La dirección está decidida, pero no es blocker para levantar las apps.
