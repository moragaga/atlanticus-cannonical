# Atlanticus — Roadmap

Estado: **CANONICAL BASELINE 1.0 — EXECUTION IN PROGRESS**

## Regla

No expandir arquitectura general sin necesidad de producto.

Cerrar verticalmente capacidades integrables y verificables.

Mantener un foco por incremento.

## Web Platform — checkpoint actual

```text
moragaga/atlanticus@384a68fe8fa42263623c95d1d132af2ca54574c8
```

Estado relevante:

```text
USERS-CANONICAL-SOURCE-1                      CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-PROJECTION-2                  CLOSED / VERIFIED / CURRENT
PROFILES-DOMAIN-EXTRACTION                    CLOSED / VERIFIED / CURRENT
PROFILES-BASELINE-SEMANTICS                   CLOSED / VERIFIED / CURRENT
UCS-1 CANONICAL-CONTRACT-SPLIT                CLOSED / VERIFIED / CURRENT
ADMIN-COMPOSITION-BACKEND                     CLOSED / VERIFIED / CURRENT
MANAGER-EXACT-SOURCE-BOUNDARY                 CLOSED / VERIFIED / CURRENT
USERS-PROFILES-ADMIN-DRAFT-BASELINE-SEMANTICS CLOSED / VERIFIED / CURRENT
USERS-MANAGER-EXACT-SOURCE-COMPOSITION        CLOSED / VERIFIED / CURRENT
ADMIN-UI-DRAFT-CUTOVER                        CLOSED / VERIFIED / CURRENT
USERS-EXACT-MANAGER-LIFECYCLE                 CLOSED / VERIFIED / CURRENT

USERS-PROFILES-DOMAIN-SEPARATION              IN PROGRESS
USERS-PROFILES-ADMIN-COMPOSITION              IN PROGRESS

ADA-LEGACY-PROJECTION-CONTRACT-ALIGNMENT       PLANNED / NEXT
USERS-RUNTIME-CANONICAL-CUTOVER               PLANNED
USERS-RUNTIME-EXACT-RELEASE-PROVENANCE        PLANNED
DOMAIN-LEGACY-DELETION                        BLOCKED
```

## Cierre Users exact Manager lifecycle

Queda congelado:

```text
validate        EXACT
read            EXACT
publish         EXACT
status          EXACT
project         EXACT
history list    EXACT
history read    EXACT
history preview EXACT
history -> work EXACT
legacy workflow NONE
```

Además:

- `UsersManagerWorkflowAdapter` fue removido;
- Users `ManagerModule.workflow_service=None`;
- exact Source reader/publication/history son servicios separados;
- exact Projection status/target/result conserva modelos `projection/core`;
- History conserva `HistoryPage` + `SourceReleaseRef`;
- preview Users usa `UsersProfilesConfiguration`;
- historical load crea trabajo local sobre BASE current;
- no se repunta Source current;
- no existen shims release/string.

`USERS-MANAGER-PRODUCTIVE-EXACT-SOURCE-CUTOVER` queda SUPERSEDED por este cierre más preciso.

## Qualification actual

```text
focused Manager + Users Configuration + users-manager   238 passed
full ADA                                                56 passed / 4 failed
```

Los cuatro failures ADA restantes pertenecen a adapters Projection legacy de Navigation/Tools/KPI/KPI Definitions.

No son un motivo para reabrir Users.

## Siguiente foco aislado recomendado

```text
ADA-LEGACY-PROJECTION-CONTRACT-ALIGNMENT
PLANNED / NEXT
```

Objetivo de debate/diseño:

- auditar el contrato `ConfigurationLifecycleWorkflow` vigente;
- alinear `get_current_projection_target()`;
- reemplazar `project(expected_source_revision: str)` por `project(ProjectionTarget)` donde corresponda;
- construir `ProjectionExecutionResult` con `target`;
- mantener History/publication legacy sólo donde aún sean contratos válidos;
- no inventar exact-source para dominios que aún no lo requieren;
- conservar cambios pequeños por adapter/capability.

Affected:

```text
NavigationManagerWorkflowAdapter
ToolConfigurationManagerWorkflowAdapter
KpiConfigurationManagerWorkflowAdapter
KpiDefinitionManagerWorkflowAdapter
```

## No mezclar con el siguiente foco

- Users exact lifecycle;
- Users runtime canonical cutover;
- Users runtime exact-release provenance;
- Root physical config;
- Python 3.14.7 global migration;
- Navigation administrative migration general;
- Manager IndexedDB global;
- legacy deletion global;
- Docker E2E general.

## Después, como incrementos independientes

```text
USERS-RUNTIME-CANONICAL-CUTOVER           PLANNED
USERS-RUNTIME-EXACT-RELEASE-PROVENANCE    PLANNED
NAV-CONSUMER-MIGRATION-B                  PLANNED
DOMAIN-LEGACY-DELETION                    BLOCKED
```

`USERS-ADMIN-CANONICAL-MIGRATION` permanece IN PROGRESS como umbrella hasta retirar o adjudicar consumidores legacy fuera del Manager Users ya migrado.

## Profile delete replacement UX

Backend contract CLOSED: delete de Profile referenciado exige replacement y reasignación atómica.

UI actual bloquea/no ejecuta ese delete cuando hay referencias.

La UX para elegir replacement permanece PLANNED y no reabre el backend contract.

## Runtime canonical cutover

Permanece PLANNED.

Debe materializar `users.runtime` desde contrato/projection canónico.

## Runtime exact-release provenance

Permanece PLANNED.

No introducir shim `SourceReleaseId <-> str`.

## Productive dependency construction / E2E

UNVERIFIED:

- assembly externo concreto de `users_profiles_administration`;
- assembly externo concreto de `users_exact_projection`;
- Docker E2E del host completo.

No inferir estas capas desde la composición de aplicación ya cerrada.

## Python/Trixie

Python 3.14.7 + Trixie permanece decidido y pendiente global.

No mezclar esta migración con el siguiente foco.

## Resource readiness

El resource físico de `users.runtime` está congelado.

La inventory global y resource topology del canonical Users Projection store continúan abiertos.

No inferir `profiles.runtime`.
