# Atlanticus — Roadmap

Estado: **ROADMAP POR FRENTE — checkpoints anteriores preservados + delta Manager M01 / ADA M02 (2026-09-29)**. Ningún NEXT de un frente constituye prioridad universal.

## Checkpoint KPI publicado — histórico para este cierre

```text
moragaga/atlanticus@d484569cbe0290f38f239481cde81b13a23deecf

KPI-RUNTIME-REPROCESS-CURRENT                 CLOSED / checkpoint anterior
KPI-DELIVERY-REGISTRY-CONSUMPTION            CLOSED / checkpoint anterior
KPI-TIMESERIES-REGISTRY-CONSUMPTION          CLOSED / checkpoint anterior
KPI-HISTORIAN-REPROCESS-CURRENT              CLOSED / checkpoint anterior
ATLANTICUS-WEB-OBSERVABILITY-SERVICE         CLOSED / checkpoint anterior
ADA-WEB-KPI-COLLECTOR-CAPABILITY             CLOSED / checkpoint anterior
KPI-COLLECTOR-DEFINITION-ATTACHMENT          CLOSED / checkpoint anterior
KPI-COLLECTOR-REAL-WEB-SMOKE                 CLOSED / checkpoint anterior

ADA-GENERIC-COLLECTOR-OPERATIONAL-INTEGRATION PLANNED / NEXT DEL FRENTE KPI
```

El NEXT del frente KPI es montar el Collector ya implementado **en la composición operacional existente**, no rediseñarlo ni crear otra aplicación. Buscar la ToolConfiguration y su proyección en código CURRENT, reutilizar Cosmos actual y montar el collector mediante `attach_ada_kpi_collector(existing WebApplicationDefinition, collector)` antes de `create_web_application`. Validar ToolStructure real, inicio del poller solo a petición operacional y llegada de Latest/Timeseries a browser stores.

Frentes separados de KPI: `KPI-INSPECTION-DEFINITION-PROVIDER-REALIGNMENT` OPEN; `PYTHON-METADATA-ALIGNMENT` OPEN / SEPARATE; `FULL-BACKEND-PYTEST-TOPOLOGY` BLOCKED / SEPARATE. Este hito Manager no los modifica ni los recalifica.

## Frente separado: ADA Datos operacionales

**Estado CURRENT de dominio y persistencia:** código de ADA contiene catálogo, Source por usuario, proyecciones Cosmos, validaciones de cargo y UI propia bajo `ManagerEntry`. El hito anterior comprobó código y registró 25 pruebas operacionales PASS del usuario y otro gate Manager de 13 PASS. No reproducir esos recuentos como qualification M01.

**Delta M01 — CLOSED / CURRENT:** `moragaga/atlanticus:main@9cc2cebe595ef1341830374ad2bb3c61baf6f5a2` está publicado y contiene solo extensión genérica de vista complementaria de Manager, 11 archivos modificados y un test nuevo. Gate local M01: **82 Manager PASS**, espejos AST PASS, `git diff --check` PASS; seis avisos Ruff previos permanecen. No CI ni validación visual ADA.

```text
OPERATIONAL-DOMAIN-AND-INDIVIDUAL-SOURCES       CLOSED / CURRENT
OPERATIONAL-COSMOS-PROJECTIONS                CLOSED / CURRENT
OPERATIONAL-EXISTING-MANAGER-UI                CURRENT / PRE-M02
MANAGER-COMPANION-M01                         CLOSED / CURRENT
ADA-OPERATIONAL-M02                           PLANNED / NEXT SOLO DE ESTE CHAT
OPERATIONAL-SNAPSHOT-TECH-CONTRACT            PLANNED / OPEN / SEPARATE
OPERATIONAL-SNAPSHOT-IMPLEMENTATION           PLANNED / BLOCKED BY CONTRACT
OPERATIONAL-SESSION-INDIVIDUAL-COSMOS          PLANNED / SEPARATE
OPERATIONAL-PROFILES-CATALOG-WARMUP            PLANNED / SEPARATE
```

**Foco único tras este cierre:** M02, adaptar `Datos operacionales` a `ManagerModule`/`ManagerCompanionView` y consumir **los workflows de catálogo ya existentes**, con asignaciones individuales inmediatas. Antes de codificar, reconciliar la propuesta posterior «Asignaciones» / «Catálogo de cargos» con el canonical previo «Datos operacionales» / «Asignación». Evitar selector duplicado, publicar cargos fuera de workspace y modificar dominios genéricos.

**Snapshot funcional DECIDED / técnico OPEN:** archivo único durable sobrescrito y sin versiones propias, solo usuarios con algún atributo operacional no nulo y exclusivamente para recuperación conjunta; nunca superficie de consumo apps/workers. No confundirlo con Source individuales ni convertirlo en requisito de M02. El diseño del snapshot, la integración Entra/sesión y el warmup de Profiles + catálogo son incrementos independientes. Ver `10_MANAGER/11_ADA_OPERATIONAL_DATA_ROADMAP.md`.
