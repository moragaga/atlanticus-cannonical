# ADA Generic — Current Composition

Estado: **CURRENT — PRODUCT COMPOSITION ROOT / SHARED MASTER PROJECTION**

## Version CURRENT

```text
ada-generic-application==0.2.22
atlanticus-web-master-projection==0.1.0
Python == 3.14.2
```

## Composition root

ADA Generic posee:

```text
settings
local/durable Manager composition
identity binding
Tool Projection resolution
operational render binding
ADA Master Projection composition/provisioning
KPI Collector attachment
Web runtime lifecycle
```

## Master Projection

Reusable engine:

```text
web/capabilities/master-projection
atlanticus.web.master_projection
```

ADA-specific ownership retained:

```text
ada.web.application.generic.master_projection.composition
ada.web.application.generic.master_projection.provision
```

SUPERSEDED:

```text
ADA-local copies of:
apply
material
plan
reader
web
```

No recrearlas.

## Persistence modes

```text
ADA_PERSISTENCE_MODE=local
→ local Source / Projection / Manager
→ local Master material

ADA_PERSISTENCE_MODE=durable
→ Blob Source / Manager / Master material
→ Cosmos Projection / Manager
```

`ATLANTICUS_ENVIRONMENT=local` puede combinarse con durable persistence.

## Current Source/namespace dependency

ADA sigue consumiendo:

```text
ada-web-storage-namespace
AdaStorageNamespace(application_namespace, tool_namespace)
```

El próximo frente decidirá si esa capability debe extraerse a Atlanticus para consumo común con
Command Center.

No cambiar el contrato Source Core por ese motivo.

## Qualification

```text
pytest   249 passed
Ruff     PASS
format   PASS
AST mirror host PASS
```
