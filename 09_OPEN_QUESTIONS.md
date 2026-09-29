# Atlanticus — Open Questions

Estado: **CANONICAL OPEN ITEMS**

## CLOSED — KPI backend

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

## CLOSED — Collector capability

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

Ya no están OPEN:

```text
exact numeric intervals
Latest/Timeseries server coherency policy
browser store ownership
component/destination mapping
poller lifecycle
Web observability integration
WebApplicationDefinition attachment
```

## OPEN — Collector operational integration

```text
ADA-GENERIC-COLLECTOR-OPERATIONAL-INTEGRATION
PLANNED / NEXT
```

Pregunta operacional única:

```text
¿Dónde y cómo se compone CURRENT la aplicación operacional real que posee ToolConfiguration,
Tool projection revision y Cosmos configuration/client para poder instanciar AdaKpiCollector y
aplicar attach_ada_kpi_collector?
```

No responder desde memoria ni creando una aplicación nueva. Inspeccionar `atlanticus:main` y
usar el composition root real.

Acceptance a cerrar allí:

```text
collector realmente montado en la aplicación operacional
ToolStructure real alimenta component stores
reader usa Cosmos configuration CURRENT
health sigue sin arrancar poller
request operacional inicia poller
Latest/Timeseries llegan a stores de lectura browser
```

## OPEN — KPI Inspection stale Definition consumer

```text
KPI-INSPECTION-DEFINITION-PROVIDER-REALIGNMENT
OPEN / SEPARATE
```

## OPEN — Python metadata

```text
Project baseline = Python 3.14.7
some package metadata observed = 3.14.2
PYTHON-METADATA-ALIGNMENT = OPEN / SEPARATE
```

## BLOCKED / SEPARATE — full backend test topology

```text
collection failures around tests.support
UNVERIFIED AS PREEXISTING
```

## PROPOSED / DEFERRED

```text
Latest Delivery REPROCESS_CURRENT
Timeseries Delivery REPROCESS_CURRENT
Historian reprocess_from optimization
```

---

## OPEN — ADA Datos operacionales, 2026-09-29 (frente independiente)

1. **BLOCKED hasta decisión de diseño:** «snapshot único» en Blob: ¿vista consolidada **adicional** reconstruible conservando Source por usuario, o reemplazo total del modelo individual? Definir schema, versionado, concurrencia, recuperación y disparadores.
2. **BLOCKED hasta decisión de dominio:** criterio del conjunto de usuarios del snapshot: promovidos actuales, deshabilitados y/o retirados. `UsersAdministrationStore.list_users()` no es índice histórico de todos los Source publicados.
3. **OPEN / UNVERIFIED:** «un registro en Cosmos por cada cambio» no es lo que implementa el store actual, que mantiene una proyección vigente por usuario. Confirmar si se requiere un histórico append-only en Cosmos, distinto del historial de Source.
4. **OPEN / QUICK FIX:** Ruff `I001` en `operational-identification/service.py` y espejo comentado; repetir tests y Ruff.
5. **PLANNED:** nuevo orden UI «Datos operacionales» / «Asignación», estado/trazabilidad conforme al Manager genérico, pruebas funcionales y validación visual.
6. **PLANNED:** integración de Cosmos con sesión Entra, guest → promoción → recarga → lectura individual, sin precarga de usuarios en warmup.
7. **PLANNED:** warmup exclusivo de catálogo Profiles y catálogo operacional, refresco periódico configurable (10 min propuesto), estados de degradación. Usuarios/asignaciones **excluidos**.
8. **UNVERIFIED:** Azure/Entra end-to-end, multi-worker real y metadata Python 3.14.7 del scope (el archivo consultado requiere `==3.14.2`).

Referencia y orden de trabajo: `10_MANAGER/11_ADA_OPERATIONAL_DATA_ROADMAP.md`. Estos abiertos **no sustituyen** el foco KPI descrito arriba.
