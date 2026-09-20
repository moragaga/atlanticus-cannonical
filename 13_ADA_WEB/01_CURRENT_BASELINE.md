# ADA Web — Current Baseline

Estado: **CURRENT**

## Implementación auditada

```text
moragaga/atlanticus@d71e94d12fa31a986b3ecc0262fbbb6ef2e4a3dd
```

## KPI Registry

```text
scopes/ada/web/kpis/registry/
├── core
├── configuration
├── projection-local
└── projection-cosmos
```

Packages:

```text
ada-web-kpi-registry==0.1.0
ada-web-kpi-registry-configuration==0.1.0
ada-web-kpi-registry-projection-local==0.1.0
ada-web-kpi-registry-projection-cosmos==0.1.0
```

Projection:

```text
ProjectionRecord[KpiRegistry]
```

Cosmos contract:

```text
logical_id = ada.kpis.registry.projection
physical   = ada-kpi-registry-projection
document_type = ada_kpi_registry_projection_record
```

## KPI Definition

```text
scopes/ada/web/kpis/definition/
├── core
├── configuration
├── projection-local
└── projection-cosmos
```

Packages:

```text
ada-web-kpi-definition==0.6.0
ada-web-kpi-definition-configuration==0.1.0
ada-web-kpi-definition-projection-local==0.1.0
ada-web-kpi-definition-projection-cosmos==0.1.0
```

Projection:

```text
ProjectionRecord[KpiDefinitionCatalog]
```

Cosmos contract:

```text
logical_id = ada.kpis.definition.projection
physical   = ada-kpi-definition-projection
document_type = ada_kpi_definition_projection_record
```

## Dependency chain

```text
Tool ProjectionTarget
        ↓
KPI Registry ProjectionTarget
        ↓
KPI Definition ProjectionTarget
```

## Configuration Manager local runtime

Registry y Definition usan projection stores locales durables.

No usan in-process projection para esas dos capabilities.

## UI invariant

Durante ambos cutovers:

```text
CSS
css.list
IDs
```

fueron preservados byte a byte.

No interpretar namespace/import changes como cambios visuales.

## Python

Project baseline:

```text
3.14.7
```

Packages CURRENT observados:

```text
requires-python ==3.14.2
```

Clasificación:

```text
PYTHON-METADATA-ALIGNMENT
OPEN / SEPARATE
```

## Conflict separado

KPI Inspection Definition provider aún consume un Definition contract histórico.

No forma parte del baseline KPI Registry/Definition cerrado.
