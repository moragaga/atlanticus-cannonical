# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

## Autoridad

Implementación publicada y verificada por inspección:

```text
moragaga/atlanticus@4c7f8aa8b541e8b8f8abc7b49fe22526a4952bfe
```

Parent inmediato:

```text
27c2e4beed125fe379881048f0df5fbe3ff6cb1a
```

## Estado resumido

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER          CLOSED / VERIFIED / CURRENT
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER           CLOSED / VERIFIED / CURRENT
USERS-MANAGER-GENERIC-CONTRACT-CUTOVER             CLOSED / VERIFIED / CURRENT
USERS-CLEAN-CUTOVER-COMPLETION                     CLOSED / VERIFIED / CURRENT
USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL        CLOSED / VERIFIED / CURRENT
PROJECTION-CORE-STALE-TEST-ALIGNMENT               CLOSED / VERIFIED
TOOLS-GENERIC-SOURCE-PROJECTION-CUTOVER            CLOSED / VERIFIED / CURRENT
KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER       CLOSED / VERIFIED / CURRENT
KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER   PLANNED / NEXT
ADA-CONFIGURATION-MANAGER-FINAL-CUTOVER            BLOCKED
MANAGER-CONSUMER-GLOBAL-QUALIFICATION              BLOCKED
```

## VERIFIED

### Ownership ADA

Tools, KPI Configuration y KPI Definition son capacidades ADA-specific bajo `scopes/ada`.

Consumir infraestructura genérica Atlanticus no mueve automáticamente una capability hacia `web/capabilities`.

KPI Configuration permanece en:

```text
scopes/ada/web/kpis/configuration
```

### KPI Configuration Source CURRENT

```text
KpiSourceCodec
KpiSourcePayload
KpiSourceRelease
KpiSourceService
SourceStore
SourceSnapshot
SourceReleaseRef
PublishRequest
PublishResult
HistoryQuery
HistoryPage
```

Publication usa:

```text
expected_concurrency_token
basis_release
```

No usa `expected_source_revision` ni una revision string de dominio como identidad Source.

### KPI Configuration Projection CURRENT

```text
KpiProjectionBuilder
create_kpi_projection_service(...)
ProjectionStore[KpiConfiguration]
SourceProjectionService[KpiConfiguration]
ProjectionTarget
```

La selección de target incorpora exactamente una dependencia Tool mediante `ProjectionTarget.dependencies`.

Durante `project(target)`, el builder vuelve a cargar el snapshot de destinos y exige igualdad exacta con la dependencia Tool seleccionada. Si Tool cambió entre selección y ejecución, la proyección falla en lugar de usar silenciosamente el Tool más nuevo.

### Destination boundary CURRENT

```text
KpiDestination
KpiDestinationCatalog
KpiDestinationCatalogSnapshot
KpiDestinationCatalogProvider
```

`KpiDestinationCatalog` conserva semántica de dominio.

La procedencia temporal pertenece a:

```text
KpiDestinationCatalogSnapshot.projection_target
```

No existe `tool_projection_revision` como identidad privada CURRENT.

### Legacy removido de KPI Configuration

Removidos de producción y mirror comentado:

```text
contracts.py
lifecycle.py
projection.py
services.py
source.py
```

Removidos tests legacy:

```text
test_projection.py
test_services.py
test_source.py
```

No quedan en `src`, `commented` ni `tests` los tokens legacy buscados durante el cierre:

```text
tool_projection_revision
source_revision
projection_revision
expected_source_revision
build_kpi_configuration_digest
KpiConfigurationSourceDocument
KpiConfigurationProjectionWorkflow
```

### Qualification local observada

```text
Python runtime shown by shell: 3.14.7
uv lock: PASS
uv sync --group dev: PASS
uv run ruff check src tests: PASS
uv run pytest: 45 passed
git diff --check: PASS
legacy token scan: 0 matches after build/cache cleanup
```

El commit publicado `4c7f8aa8...` fue inspeccionado después y contiene el cutover.

## INFERRED

El patrón CURRENT para KPI Definition debe reutilizar los contratos genéricos Source/Projection donde corresponda, preservando sus semánticas propias y su ownership ADA.

Esto no autoriza copiar KPI Configuration ciegamente ni asumir rutas, schemas, providers o consumers de KPI Definition sin inspeccionar su implementación CURRENT.

## ASSUMED

No se asume:

- composición física final que suministra `KpiDestinationCatalogProvider` en producción;
- estado actual interno de KPI Definition antes de inspeccionarlo;
- necesidad de migración operacional de datos KPI históricos;
- full ADA regression;
- Docker E2E;
- CI remoto;
- que el pin Python de todos los packages ya esté alineado con el baseline global.

## PROPOSED / NEXT

Único foco siguiente:

```text
KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
PLANNED / NEXT
```

Primera etapa solamente: debate y diseño contra `atlanticus:main` y canonical CURRENT.

## SUPERSEDED

```text
KPI Configuration Source/Projection private lifecycle
SUPERSEDED / REMOVED

private revision strings as Source/Projection/dependency identity
SUPERSEDED / REMOVED

base KPI Configuration Projection is independent of Tool Projection
SUPERSEDED / REFINED

compatibility adapters/shims to preserve KPI legacy
SUPERSEDED / FORBIDDEN

move KPI Configuration into generic core because it consumes generic infrastructure
SUPERSEDED / FORBIDDEN
```

La regla refinada es:

```text
no global rigid projection order
+
real semantic projection dependencies are represented by ProjectionTarget.dependencies
```

KPI Configuration tiene una dependencia real y exacta en Tool Projection.

## UNVERIFIED / OPEN

- KPI Definition Source/Projection cutover;
- concrete production composition/provider wiring for KPI destination snapshots;
- full ADA suite;
- final Configuration Manager regression;
- Docker E2E;
- CI remoto del checkpoint `4c7f8aa8...`;
- Python 3.14.7/Trixie global qualification;
- alignment del `requires-python` de KPI Configuration con el baseline global.

## Conflicto de baseline Python

Canonical fija:

```text
Python 3.14.7
```

La implementación publicada de KPI Configuration todavía declara:

```text
requires-python = "==3.14.2"
```

Esto queda OPEN y fuera del foco KPI Definition salvo que impida ejecutar su qualification.

## Siguiente frontera

```text
KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
PLANNED / NEXT
```

No mezclar Configuration Manager final, Python baseline cleanup, Command Center, Operational Data ni rediseño de Projection core.
