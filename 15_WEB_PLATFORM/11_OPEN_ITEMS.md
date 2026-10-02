# Web Platform — Open Items

Estado: **CURRENT — DUAL-APP DURABLE RUNTIME SMOKE NEXT**

## CLOSED / VERIFIED

```text
atlanticus-web-master-projection extraction
ADA adoption of shared Master engine
Command Center adoption of shared Master engine
Command Center durable Manager composition
dual .env.detail alignment
SOURCE-NAMESPACE-AND-COMPOSITION-CONVERGENCE
```

## Namespace convergence — CLOSED

Implemented generic capability:

```text
web/capabilities/storage/namespace
atlanticus-web-storage-namespace==0.1.0
```

Frozen public type:

```text
StorageNamespace(
    application_namespace,
    scope_namespace,
)
```

Validated consumers:

```text
ADA Tool Projection Local
ADA Tool Projection Cosmos
ADA Tool Persistence
ADA Generic Application
Command Center Tool Discovery Cosmos
Command Center Tool Catalog Manager
Command Center Configuration Manager
Command Center Generic Application
```

Qualification:

```text
370 relevant tests passed
Ruff PASS
format PASS
mirrors PASS
legacy namespace references = 0
9 uv lock --check PASS
```

## OPEN / NEXT

```text
DUAL-APP-DURABLE-RUNTIME-SMOKE
```

Required:

```text
agree exact selected local/durable targets
start ADA Generic with durable composition
start Command Center with durable composition
validate Storage/Cosmos bindings
validate namespace-derived physical identities at runtime
validate restart/readback where the smoke contract requires it
```

Do not mix distribution regeneration into the same increment.

## OPEN / AFTER

```text
CURRENT-HEAD-DISTRIBUTION-REGENERATION
ADA-GENERIC-OVER-ATLANTICUS-DISTRIBUTION
```

After ADA is proven against distribution, resume ADA backend completion:

```text
KPI Runtime
KPI Historian
KPI Delivery / Timeseries
Collector
browser stores
UI
```

## OPEN / CONDITIONAL

```text
COMMAND-CENTER-RESOURCE-PREPARATION-PARITY
```

Only if durable runtime evidence shows it is required.

## OPEN / SEPARATE

```text
production Entra
Azure production qualification
Python 3.14.7 / Trixie
scope tooling topology normalization
Users destructive recovery
other historical Web gates not revalidated here
```

Historical Source-ownership NEXT markers are superseded by the current durable-runtime ordering.
