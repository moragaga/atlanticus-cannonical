# ADA Generic — Configuration to Runtime

Estado: **FROZEN SEMANTICS + GENERIC CONTRACT MIGRATION IN PROGRESS**

## Cadena de autoridad

La cadena funcional es:

```text
Tool Configuration Source
    ↓
Tool Projection
    ↓
KPI Destination Catalog
    ↓
KPI Configuration Source
    ↓
KPI Configuration Projection
    ├── Delivery policy
    └── KpiCatalog
            ↓
KPI Definition Authority
            ↓
KPI Definition Source
            ↓
KPI Definition Projection
            ↓
Operational Render
            ↓
Alarm visual state
```

## Estado de contratos

```text
Tool Source/Projection
CLOSED / CURRENT

KPI Configuration Source/Projection
PLANNED / NEXT

KPI Definition Source/Projection
PLANNED
```

## Tool CURRENT

Tools permanece ADA-specific pero usa directamente:

```text
SourceStore
SourceSnapshot
SourceReleaseRef
ProjectionTarget
ProjectionStore[ToolConfiguration]
SourceProjectionService[ToolConfiguration]
```

No existe private Tool lifecycle/revision identity en el contrato CURRENT.

## Dependencias entre projections

La dependencia semántica no desaparece al migrar contratos.

```text
KPI Configuration Projection
    depends on exact Tool Projection target

KPI Definition Projection
    depends on exact KPI Configuration Projection target
```

La identidad final debe usar `ProjectionTarget`/dependencies genéricos.

No usar como identidad final:

```text
tool_projection_revision
kpi_configuration_revision
other private revision strings
```

Las propiedades de dominio que no sean identidad Projection se preservan cuando estén justificadas por semántica real.

## Regla maestra

`CONFIGURATION DETERMINES EXISTENCE`

`DATA DETERMINES STATE`

La primera medición no puede ser el evento que crea la UI.

## Arranque vacío

Si la configuración declara Component/KPI:
- montar estructura;
- evaluar Store vacío;
- mostrar `EMPTY`.

No:
- esperar primer dato;
- inferir inexistencia por ausencia de dato.

## Alarmas

Data granularity != visual alarm granularity.

Las alarmas se proyectan sobre identidad estructural y pueden coexistir con dato vacío.

## Estrategia de cutover

Cada dominio Configuration se migra a su contrato final antes de cortar el consumer Manager.

```text
Tools
→ KPI Configuration
→ KPI Definition
→ ADA Configuration Manager
→ regression
```

No introducir adapters/shims para mantener el consumer ejecutable entre etapas.

## Handoff hacia Command Center

Tool Configuration tiene consumidores distintos:

```text
Tool Configuration
   ├── ADA Generic → experiencia operacional
   └── Command Center → validación/configuración de alarmas
```

Command Center no modifica Tool Configuration.

Este frente no se abre durante los cutovers KPI actuales.
