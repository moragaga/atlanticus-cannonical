# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

## Autoridad

Implementación publicada y verificada por inspección:

```text
moragaga/atlanticus@ef3f0a44c5dcc14f8fcafe5bb36bb97865381924
```

Parent inmediato:

```text
4c7f8aa8b541e8b8f8abc7b49fe22526a4952bfe
```

Canonical inspeccionado para este cierre:

```text
moragaga/atlanticus-cannonical@430a90529c99e69d16978f60d91aa86f819b851e
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
KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER   CLOSED / VERIFIED / CURRENT
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER    PLANNED / NEXT
MANAGER-CONSUMER-GLOBAL-QUALIFICATION              BLOCKED
WEB-TEST-CONTRACT-CLEANUP                          PLANNED / AFTER MANAGER
```

## VERIFIED

### Ownership ADA

Tools, KPI Configuration y KPI Definition son capacidades ADA-specific bajo `scopes/ada`.

Consumir infraestructura genérica Atlanticus no mueve automáticamente una capability hacia `web/capabilities`.

### KPI Definition Source CURRENT

```text
KpiDefinitionSourceCodec
KpiDefinitionSourcePayload
KpiDefinitionSourceRelease
KpiDefinitionSourceService
SourceStore
SourceSnapshot
SourceReleaseRef
PublishRequest
PublishResult
HistoryQuery
HistoryPage
```

No existe `expected_source_revision` ni una revision string privada como identidad Source CURRENT.

### KPI Definition Projection CURRENT

```text
KpiDefinitionProjectionBuilder
create_kpi_definition_projection_service(...)
ProjectionStore[KpiDefinitionCatalog]
SourceProjectionService[KpiDefinitionCatalog]
ProjectionTarget
```

Dependencia directa:

```text
ProjectionStore[KpiConfiguration]
+
exact KPI Configuration ProjectionTarget
```

El target KPI Definition contiene exactamente una dependencia KPI Configuration. El builder carga la proyección activa KPI Configuration y exige igualdad exacta con la dependencia seleccionada. Si cambió antes de ejecutar KPI Definition, la proyección falla.

### Semántica de cobertura CURRENT

```text
KpiDefinitionCatalog
KpiDefinitionCoverageStatus.DEFINED
KpiDefinitionCoverageStatus.MISSING
```

Un KPI configurado sin Definition es `MISSING` y sigue siendo una proyección válida.

Una Definition cuyo `kpi_key` no existe en `KpiConfiguration.kpi_keys` es inválida para proyección.

### Legacy removido de KPI Definition

El paquete CURRENT ya no exporta ni usa como contrato de dominio:

```text
KpiDefinitionAuthorityCatalog
KpiDefinitionAuthorityProvider
KpiDefinitionAdministrationService
KpiDefinitionProjectionWorkflow
KpiDefinitionServices
KpiDefinitionSourceDocument
build_kpi_definition_digest
build_kpi_definition_projection_revision
expected_source_revision
```

El contrato final usa Source/Projection genéricos directamente.

### Qualification local observada

```text
Python runtime shown by shell: 3.14.7
uv lock: PASS
uv sync --group dev --extra web: PASS
uv run ruff check src tests: PASS
uv run pytest: 40 passed
legacy token scan scoped: 0 matches
```

El commit publicado `ef3f0a44...` fue inspeccionado después y contiene el cutover.

### Configuration Manager consumer mismatch CURRENT

`scopes/ada/web/application/ada-configuration-manager` permanece en el contrato anterior.

Verificado en `main@ef3f0a44...`:

```text
ConfigurationManagerDependencies
    imports KpiConfigurationServices
    imports KpiDefinitionServices
    imports KpiDefinitionAuthorityProvider
    imports ToolLifecycleServices
    imports ExactProjectionWorkflow
    imports NavigationConfigurationServices

composition.py
    imports old Manager workflow adapters
    imports create_users_manager_exact_source_* names
    constructs ManagerModule with workflow_service / exact_source_* fields

workflows.py
    maps source_revision / projection_revision
    publishes with expected_source_revision
    projects from revision strings
```

Al mismo tiempo, Manager CURRENT exige:

```text
ManagerModule
├── source_key
├── source_service
├── source_reader_service
├── projection_service
├── draft_validation_service
└── source_history_service | None
```

`atlanticus.web.manager` ya no exporta `ExactProjectionWorkflow`, y `atlanticus.web.compositions.users_manager` ya no exporta `create_users_manager_exact_source_*`.

Esto es un conflicto de consumer CURRENT, no una razón para reintroducir legacy en los dominios ya migrados.

## INFERRED

Como Users, Navigation, Tools, KPI Configuration y KPI Definition ya tienen contratos finales, el corte correcto siguiente es refactorizar `ada-configuration-manager` completo una sola vez al contrato Manager genérico.

No conviene hacer un mini-cutover exclusivo de KPI Definition y volver a tocar el mismo composition root después.

## ASSUMED

No se asume todavía:

- composición final exacta de services por módulo dentro de `ada-configuration-manager`;
- qué clases de composición pueden conservarse con responsabilidad legítima;
- alcance exacto de los cambios Web internos del consumer;
- que toda la suite ADA pase con el consumer actual;
- Docker E2E;
- CI remoto;
- Python 3.14.7/Trixie global qualification;
- que la metadata `requires-python` ya esté alineada.

## PROPOSED / NEXT

Único foco siguiente:

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
PLANNED / NEXT
```

Primera etapa: inspección y diseño del consumer completo contra contratos CURRENT. No implementar hasta cerrar trazabilidad, contratos, archivos y tests afectados.

## SUPERSEDED

```text
KPI Definition private Authority bridge
SUPERSEDED / REMOVED

KPI Definition private Source/Projection lifecycle
SUPERSEDED / REMOVED

private revision strings as Source/Projection/dependency identity
SUPERSEDED / REMOVED

KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER as next work
SUPERSEDED / CLOSED

ADA Configuration Manager blocked by KPI Definition migration
SUPERSEDED / UNBLOCKED

migrar sólo el consumer KPI Definition antes del Manager final
SUPERSEDED / REFINED
```

La regla refinada es:

```text
all Configuration domain contracts first
→ one final ada-configuration-manager cutover
→ global regression
```

## UNVERIFIED / OPEN

- final generic cutover de `ada-configuration-manager`;
- full ADA suite después del cutover final;
- Docker E2E;
- CI remoto de `ef3f0a44...`;
- Python 3.14.7/Trixie global qualification;
- alignment de `requires-python` en KPI Configuration y KPI Definition;
- concrete production composition/provider wiring para KPI destination snapshots, si sigue siendo relevante tras inspeccionar el consumer final;
- revisión transversal posterior de tests Web de estructura interna/existencia/CSS donde existan.

## Conflicto de baseline Python

Canonical fija:

```text
Python 3.14.7
```

La implementación publicada declara:

```text
KPI Configuration: requires-python ==3.14.2
KPI Definition:    requires-python ==3.14.2
```

La suite scoped de KPI Definition pasó con shell Python 3.14.7, pero la metadata sigue desalineada y permanece OPEN.

## Siguiente frontera

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
PLANNED / NEXT
```

No mezclar todavía Python baseline cleanup, Command Center, Operational Data ni la limpieza transversal de tests Web.
