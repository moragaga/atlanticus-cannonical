# ADA Generic — Configuration to Runtime

Estado: **FROZEN SEMANTICS + CONFIGURATION CONTRACTS CURRENT**

## Cadena de autoridad

Cadena funcional CURRENT verificada para Configuration:

```text
Tool Configuration Source
    ↓
Tool Projection
    ↓ exact ProjectionTarget dependency
KPI Configuration Source
    ↓
KPI Configuration Projection
    ↓ exact ProjectionTarget dependency
KPI Definition Source
    ↓
KPI Definition Projection
    ↓
Operational Render
    ↓
Alarm visual state
```

El catálogo de destinos KPI forma parte de la semántica de KPI Configuration, pero la identidad temporal de su dependencia Tool se transporta mediante el `ProjectionTarget` exacto de Tool.

No existe `KpiDefinitionAuthority` como frontera CURRENT entre KPI Configuration y KPI Definition.

## Estado de contratos

```text
Tool Source/Projection
CLOSED / VERIFIED / CURRENT

KPI Configuration Source/Projection
CLOSED / VERIFIED / CURRENT

KPI Definition Source/Projection
CLOSED / VERIFIED / CURRENT

ADA Configuration Manager generic consumer cutover
CLOSED / VERIFIED / CURRENT
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

Frontera exacta:

```text
Tool ProjectionTarget
    ↓
KPI Configuration ProjectionTarget.dependencies
```

## KPI Definition CURRENT

KPI Definition usa directamente:

```text
KpiDefinitionSourceService
KpiDefinitionProjectionBuilder
ProjectionStore[KpiDefinitionCatalog]
ProjectionStore[KpiConfiguration]
SourceProjectionService[KpiDefinitionCatalog]
ProjectionTarget
```

Frontera exacta:

```text
KPI Configuration ProjectionTarget
    ↓
KPI Definition ProjectionTarget.dependencies
```

El builder exige exactamente una dependencia KPI Configuration y valida que la proyección activa siga correspondiendo al target seleccionado.

Cobertura:

```text
configured KPI + Definition         -> DEFINED
configured KPI without Definition   -> MISSING
Definition outside configured KPI set -> invalid projection
```

## Dependencias entre projections

La dependencia semántica no desaparece al usar contratos genéricos.

Cadena CURRENT:

```text
Tool ProjectionTarget
    ↓
KPI Configuration ProjectionTarget
    ↓
KPI Definition ProjectionTarget
```

No usar como identidad final:

```text
tool_projection_revision
kpi_configuration_revision
source_revision
projection_revision
other private revision strings
```

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

## Estado del cutover

Los dominios Configuration ya alcanzaron contrato final:

```text
Tools               CLOSED / CURRENT
KPI Configuration   CLOSED / CURRENT
KPI Definition      CLOSED / CURRENT
```

El consumer ADA Configuration Manager también está cerrado sobre el contrato genérico:

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
CLOSED / VERIFIED / CURRENT
```

No introducir adapters/shims para conservar contratos consumer antiguos.

La integración administrativa de Profiles también es CURRENT, pero pertenece a la
composición Manager y no altera la cadena Tool -> KPI -> Definition.

## Handoff hacia Command Center

Tool Configuration tiene consumidores distintos:

```text
Tool Configuration
   ├── ADA Generic → experiencia operacional
   └── Command Center → validación/configuración de alarmas
```

Command Center no modifica Tool Configuration.

Los siguientes incrementos Manager no deben reabrir esta cadena.
