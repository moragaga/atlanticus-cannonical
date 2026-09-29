# Atlanticus — Roadmap

Estado: **ROADMAP POR FRENTE — checkpoint KPI histórico preservado + delta ADA operacional 2026-09-29**

## Checkpoint publicado

```text
moragaga/atlanticus@d484569cbe0290f38f239481cde81b13a23deecf
```

## KPI backend flow

```text
KPI-RUNTIME-REPROCESS-CURRENT
CLOSED / VERIFIED / CURRENT

KPI-DELIVERY-REGISTRY-CONSUMPTION
CLOSED / VERIFIED / CURRENT

KPI-TIMESERIES-REGISTRY-CONSUMPTION
CLOSED / VERIFIED / CURRENT

KPI-HISTORIAN-REPROCESS-CURRENT
CLOSED / VERIFIED / CURRENT
```

## Collector capability

```text
ATLANTICUS-WEB-OBSERVABILITY-SERVICE
CLOSED / VERIFIED / CURRENT

ADA-WEB-KPI-COLLECTOR-CAPABILITY
CLOSED / VERIFIED / CURRENT

KPI-COLLECTOR-DEFINITION-ATTACHMENT
CLOSED / VERIFIED / CURRENT

KPI-COLLECTOR-REAL-WEB-SMOKE
CLOSED / VERIFIED / CURRENT
```

## NEXT del checkpoint KPI — no perder esta frontera

```text
ADA-GENERIC-COLLECTOR-OPERATIONAL-INTEGRATION
PLANNED / NEXT
```

Objetivo único: **integrar el collector ya implementado** en la composición operacional real.

No volver a discutir polling, coherency, stores ni observability salvo conflicto demostrado.

El siguiente chat debe inspeccionar la fuente autoritativa para ubicar la composición que ya
resuelve la Tool y sus conexiones. Después debe hacer el wiring mínimo:

```text
ToolConfiguration CURRENT
    ↓
ToolStructure + tool projection revision
    ↓
Cosmos client/configuration CURRENT
    ↓
CosmosKpiDeliveryReader
    ↓
AdaKpiCollector
    ↓
attach_ada_kpi_collector(existing WebApplicationDefinition, collector)
    ↓
create_web_application
```

Acceptance del siguiente foco debe comprobar la aplicación operacional real, no sólo el package
collector aislado.

## Frentes separados

```text
KPI-INSPECTION-DEFINITION-PROVIDER-REALIGNMENT
OPEN / SEPARATE

PYTHON-METADATA-ALIGNMENT
OPEN / SEPARATE

FULL-BACKEND-PYTEST-TOPOLOGY
BLOCKED / SEPARATE
```

No mezclar estos frentes con la integración operacional del collector.

---

## Roadmap separado: ADA Datos operacionales — 2026-09-29

**VERIFIED:** `atlanticus@caced5d7711cf059d36ec61aecc9b3e9629bd41f` implementa catálogo y asignaciones con Source independientes, proyecciones Cosmos y Manager. El usuario reportó 25 pruebas PASS del scope y, en gate anterior separado, 13 pruebas PASS del Manager. Ruff mantiene un `I001` OPEN. No extrapolar los gates a Azure/Entra.

```text
OPERATIONAL-DOMAIN-AND-INDIVIDUAL-SOURCES        CLOSED / CURRENT
OPERATIONAL-COSMOS-PROJECTIONS                 CLOSED / CURRENT
OPERATIONAL-EXISTING-MANAGER-UI                 CURRENT / REORDER PLANNED
OPERATIONAL-RUFF-I001                          OPEN / ISOLATED
OPERATIONAL-SNAPSHOT-CONTRACT                  PLANNED / NEXT DESIGN / OPEN
OPERATIONAL-SNAPSHOT-IMPLEMENTATION            PLANNED / BLOCKED BY CONTRACT
OPERATIONAL-MANAGER-NEW-TABS                   PLANNED / SEPARATE
OPERATIONAL-PROJECTION-E2E-QUALIFICATION        PLANNED / SEPARATE
OPERATIONAL-SESSION-INDIVIDUAL-COSMOS           PLANNED / SEPARATE
OPERATIONAL-PROFILES-CATALOG-WARMUP             PLANNED / SEPARATE
```

**Decisión vigente:** el warmup solo carga **Profiles y catálogo operacional**; nunca usuarios, promociones ni asignaciones. La consulta individual ocurre al resolver la sesión Entra/Users y una promoción requiere recarga de la página. Apps y workers solo consumen Cosmos; Blob es durable/histórico. El «snapshot único» en Storage conserva cuestiones de contrato antes de implementarse.

Plan completo y gates: `10_MANAGER/11_ADA_OPERATIONAL_DATA_ROADMAP.md`. El NEXT indicado arriba sigue siendo el NEXT **del frente KPI**, no una prioridad universal frente al trabajo operacional paralelo.
