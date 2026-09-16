# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

## Autoridad

Implementación publicada y verificada por inspección:

```text
moragaga/atlanticus@27c2e4beed125fe379881048f0df5fbe3ff6cb1a
```

Parent inmediato:

```text
a065f45c55a527c96ce333705465487e95f0a737
```

No se dispone en este cierre de evidencia para afirmar working tree limpio ni ejecución de qualification scoped posterior al commit.

## Estado resumido

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER          CLOSED / VERIFIED / CURRENT
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER           CLOSED / VERIFIED / CURRENT
USERS-MANAGER-GENERIC-CONTRACT-CUTOVER             CLOSED / VERIFIED / CURRENT
USERS-CLEAN-CUTOVER-COMPLETION                     CLOSED / VERIFIED / CURRENT
USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL        CLOSED / VERIFIED / CURRENT
PROJECTION-CORE-STALE-TEST-ALIGNMENT               CLOSED / VERIFIED

TOOLS-GENERIC-SOURCE-PROJECTION-CUTOVER             CLOSED / VERIFIED / CURRENT
TOOLS-SCOPED-QUALIFICATION                          PLANNED / UNVERIFIED
TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER             PLANNED

KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER       PLANNED / NEXT
KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER   PLANNED
ADA-CONFIGURATION-MANAGER-FINAL-CUTOVER             BLOCKED
MANAGER-CONSUMER-GLOBAL-QUALIFICATION               BLOCKED
```

## VERIFIED

### Manager

Contrato vigente:

```text
ManagerModule
├── source_key
├── source_service
├── source_reader_service
├── projection_service
├── draft_validation_service
└── source_history_service | None
```

No forman parte de la frontera CURRENT:

```text
workflow_service
ExactSource*
ExactProjection* como frontera Manager
expected_source_revision
revision -> ProjectionTarget reconstruction
```

### Navigation

Navigation consume directamente Source/Projection/Manager genéricos.

```text
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

### Users

Users consume directamente el contrato Manager genérico y su clean cutover permanece cerrado.

```text
USERS-MANAGER-GENERIC-CONTRACT-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-CLEAN-CUTOVER-COMPLETION
CLOSED / VERIFIED / CURRENT

USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL
CLOSED / VERIFIED / CURRENT
```

### Tools ownership

Tools permanece bajo:

```text
scopes/ada/web/tools
```

Es ADA-specific. El cutover no trasladó su dominio a `web/capabilities`.

### Tools domain semantics

El commit `27c2e4be...` no modifica `models.py` ni `operational.py` de Tool Configuration.

Se preservan:

```text
ToolConfiguration
Tool Structure
Component / Subcomponent
Tool kind
Operational scope
Source consumption
Operational participation
Branding
ADA operational validation
```

### Tool Source CURRENT

```text
ToolSourceCodec
ToolSourcePayload
ToolSourceRelease
ToolSourceService
```

Consume modelos y store de `atlanticus.web.source` directamente.

Publication usa:

```text
expected_concurrency_token
basis_release
```

No usa revision string de dominio.

### Tool Projection CURRENT

```text
ToolProjectionBuilder
create_tool_projection_service(...)
ProjectionStore[ToolConfiguration]
SourceProjectionService[ToolConfiguration]
ProjectionTarget
```

El builder valida la configuración publicada mediante `validate_ada_operational_tool_configuration(...)`.

No existe una identidad paralela privada de Projection dentro del contrato CURRENT de Tools.

### Legacy removido de Tools Configuration

```text
contracts.py
lifecycle.py
projection.py
services.py
source.py
```

La fachada pública ya no exporta la familia `ToolLifecycle*`, snapshots privados ni helpers de revision legacy.

### Desalineación deliberada del consumer

`ada-configuration-manager` todavía importa `ToolLifecycleServices` y define `ToolConfigurationManagerWorkflowAdapter` con `expected_source_revision`.

Esa desalineación es CURRENT y temporal.

No se debe reparar desde Tools.

## INFERRED

Tools CURRENT demuestra el patrón mínimo de infraestructura que debe usarse como referencia para KPI Configuration:

```text
ADA-specific domain
→ generic Source
→ generic Projection
```

No demuestra que KPI Configuration pueda copiarse sin considerar su dependencia semántica en Tool Projection.

## ASSUMED

No se asume:

- `SourceKey` final de composición para Tools;
- provider físico final de Tool Source/Projection;
- necesidad de migración de datos Tool históricos;
- working tree limpio;
- suite Tools ejecutada;
- Configuration Manager ejecutable antes del cutover final.

## PROPOSED / NEXT

Único foco siguiente:

```text
KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
```

Regla de diseño ya fijada:

```text
KPI Configuration ownership remains ADA
Tool dependency is preserved
private tool_projection_revision identity is removed
exact dependency identity uses generic ProjectionTarget semantics
Manager is not repaired in this increment
```

## SUPERSEDED

```text
Tools Manager consumer must stay runnable during Tool cutover
SUPERSEDED

add adapters/shims to preserve old Tool lifecycle
SUPERSEDED / FORBIDDEN

move Tools into generic Atlanticus core because it uses generic infrastructure
SUPERSEDED / FORBIDDEN

private revision strings as Projection identity
SUPERSEDED
```

## UNVERIFIED

- `uv run pytest` scoped de Tools después de `27c2e4be...`;
- Ruff scoped de Tools después de `27c2e4be...`;
- full ADA suite;
- final Configuration Manager regression;
- Docker E2E;
- CI remoto para `27c2e4be...`;
- Python 3.14.7/Trixie global;
- providers físicos finales de Tool Source/Projection;
- datos Tool históricos que requieran migración operacional;
- KPI Configuration cutover;
- KPI Definition cutover.

## Siguiente frontera

```text
KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
PLANNED / NEXT
```

No mezclar Manager final, KPI Definition, Command Center, Operational Data ni rediseño de Manager core.
