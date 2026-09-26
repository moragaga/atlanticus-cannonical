# ADA Generic — First Deliverable Vertical

Estado: **CURRENT / GENERIC STAGE 1 CLOSED / LOCAL NAVIGATION QUALIFIED / TOOL GOLDEN PATH OPEN**

## Separación de entregables

La base ADA Generic, la qualification de Navigation local, el artifact distribuible y la
primera Tool real no son equivalentes.

### Stage 1 — CLOSED

```text
Tool Projection durable
→ resilient Tool resolution
→ ToolStructure
→ KPI Collector (si Tool READY y Cosmos configurado)
→ Latest / Timeseries
→ process cache
→ browser dcc.Store por ToolComponent
→ developer handoff
```

El Collector no es un componente pendiente por crear para esta frontera. No imponer
visualización Tool específica en ADA Generic.

### Navigation local — CLOSED / VERIFIED MANUAL

Con la configuración de desarrollo, el usuario pudo guardar, publicar y proyectar Navigation
y consumir sus enlaces desde Home. Después de corregir el callback, el menú funciona según
la comprobación final sobre `a6061ffe`.

Esto califica ese flujo local; **no** prueba persistencia Blob/Cosmos real tras reinicio,
identidad Entra ni distribución portable.

### Primer artifact distribuible — PLANNED / UNVERIFIED

Hay tooling de procesos (`deployment/processes/bundle.py`) y validadores por frontera.
No se ha acreditado todavía aquí que un artifact final de ADA Generic + sus dependencias
sea portable y ejecutable fuera del checkout; tampoco se ha acreditado un pipeline de
web distribution. Antes de agregar código, auditar las herramientas reales existentes.

### Golden Path de Tool — OPEN

Una Tool concreta debe recorrer, según corresponda:

```text
Tool Configuration
→ Source/Release
→ Projection/materialization
→ operational data
→ KPI processes/Latest/Timeseries
→ ADA Generic Collector + browser stores
→ concrete Tool visualization
→ E2E y artifact/distribution qualification
```

Las partes Alarm/Command Center se integran sólo donde la Tool las necesite, en su frente.
No son prerequisito universal impuesto al núcleo genérico.

## Orden de Tool congelado

```text
1. Operaciones Integradas
2. Mina
```

No elevar particularidades visuales de la primera Tool a arquitectura obligatoria.

## Siguiente frontera única

`ADA-GENERIC-WEB-ARTIFACT-DISTRIBUTION-QUALIFICATION`:
identificar la vía existente de empaquetado Web, verificar dependencias, configuración,
arranque fuera del checkout y criterios de entrega; diseñar sólo el gap demostrado.
Los procesos KPI/backend usan su propio bundler y no se reimplementan en este frente.
