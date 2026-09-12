# ADA Generic — Configuration to Runtime

Estado: **FROZEN SEMANTICS + INTEGRATION IN PROGRESS**

## Cadena de autoridad

La cadena funcional recuperada es:

Tool Configuration
    ↓
Tool Projection
    ↓
KPI Destination Catalog
    ↓
KPI Configuration
    ↓
KPI Configuration Projection
    ├── Delivery policy
    └── KpiCatalog
            ↓
KPI Definition Authority
            ↓
KPI Definition
            ↓
Operational Render
            ↓
Alarm visual state

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

## Integración vertical

Cada nueva capacidad debe cerrar su cadena:

contrato/backend
→ authority/source
→ materialización/projection
→ Alarm/KPI cuando corresponda
→ Web consumer
→ E2E

No desarrollar todos los frentes horizontalmente y unirlos al final.


## Handoff hacia Command Center

Tool Configuration tiene dos consumidores distintos:

```text
Tool Configuration
   ├── ADA Generic → experiencia operacional
   └── Command Center → validación/configuración de alarmas
```

Command Center no modifica Tool Configuration.

Alarm Engine devuelve estado operacional que ADA Generic consume visualmente.
