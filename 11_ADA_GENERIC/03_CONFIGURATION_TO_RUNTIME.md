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
CLOSED / VERIFIED / CURRENT

KPI Configuration Source/Projection
CLOSED / VERIFIED / CURRENT

KPI Definition Source/Projection
PLANNED / NEXT
```

## Ownership ADA

Tools, KPI Configuration y KPI Definition permanecen bajo `scopes/ada` porque sus semánticas son ADA-specific.

El uso de Source/Projection genéricos no cambia ese ownership.

## Tool CURRENT

Tools usa directamente contratos genéricos Source/Projection y no mantiene private Tool lifecycle/revision identity.

## KPI Configuration CURRENT

KPI Configuration usa directamente:

```text
SourceStore
SourceSnapshot
SourceReleaseRef
ProjectionTarget
ProjectionStore[KpiConfiguration]
SourceProjectionService[KpiConfiguration]
```

La frontera Tool→KPI Configuration transporta:

```text
KpiDestinationCatalogSnapshot
├── projection_target   exact Tool ProjectionTarget
└── catalog             semantic destination catalog
```

## Dependencias entre projections

La dependencia semántica no desaparece al migrar contratos.

CURRENT:

```text
KPI Configuration Projection
    depends on exact Tool Projection target
```

NEXT / FROZEN DIRECTION:

```text
KPI Definition Projection
    depends on exact KPI Configuration Projection target
```

La identidad final usa `ProjectionTarget`/dependencies genéricos.

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
Tools              CLOSED
→ KPI Configuration CLOSED
→ KPI Definition    NEXT
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
