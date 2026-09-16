# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global

| Decisión | Estado |
|---|---|
| Python 3.14.7 | DECIDED / NOT YET QUALIFIED GLOBALLY |
| `python:3.14.7-slim-trixie` | DECIDED / NOT YET QUALIFIED GLOBALLY |
| `uv`, no pip normal | CURRENT |
| `backend/` representa backend jobs; no todo Python server-side | CURRENT |
| Server-side Python con responsabilidad Web pertenece a `web/` | CURRENT |
| Connectivity es dual-use y no adquiere ownership funcional | CURRENT |
| Definir contratos antes que consumidores | CURRENT |
| Cutover raíz limpio | CURRENT |
| No crear shims/adapters/aliases temporales para legacy | FROZEN |
| Tests no son autoridad sobre contratos SUPERSEDED | FROZEN |

## Regla universal de cutover

```text
LEGACY
REMOVE

ADAPTERS / SHIMS / ALIASES
FORBIDDEN

DOBLE CONTRATO
FORBIDDEN

OLD SCHEMA READERS IN CURRENT RUNTIME
FORBIDDEN

CONTRATO FINAL
Generic Atlanticus contract only
```

Una pieza no deja de ser compatibilidad por ser read-only, privada, interna al codec, usada para historia durable o necesaria para mantener tests existentes.

Si su única responsabilidad es entender un contrato/schema eliminado, pertenece al legado y debe removerse del runtime CURRENT.

Una migración histórica necesaria debe ser una operación explícita y acotada, no compatibilidad permanente embebida en producción.

## Orden obligatorio de migración y validación

```text
1. definir/fijar contrato final
2. migrar implementación
3. eliminar legacy
4. eliminar tests cuyo único propósito sea preservar legacy
5. ejecutar qualification scoped
6. ejecutar qualification global
7. adjudicar únicamente desalineaciones del contrato final
```

FORBIDDEN:

```text
mantener legacy para hacer pasar tests
crear adapters para no romper tests
reintroducir schemas anteriores como fallback
declarar CLOSED sólo porque pytest está GREEN
```

## Source / Projection

| Decisión | Estado |
|---|---|
| Source genérico pertenece a `web/capabilities/source` | CURRENT |
| Projection exact-release pertenece a `web/capabilities/projection/core` | CURRENT |
| Release identity != content hash | FROZEN |
| Source current nunca lo determina Cosmos | FROZEN |
| Projection target = `SourceKey + SourceReleaseRef` | FROZEN |
| `project(target)` no relee current | FROZEN |
| CURRENT/OUTDATED compara release identity | FROZEN |
| Retry conserva exact target | FROZEN |
| No introducir shim `SourceReleaseId <-> str` | FROZEN |
| Restore publica una nueva release; no repunta current | FROZEN |
| No reconstruir `ProjectionTarget` desde revision | FROZEN |
| `expected_source_revision` | SUPERSEDED / REMOVED |

## Manager generic contract

| Decisión | Estado |
|---|---|
| Manager tiene un solo contrato Source/Projection genérico | FROZEN / IMPLEMENTED |
| `ManagerModule.source_service` | FROZEN / IMPLEMENTED |
| `ManagerModule.source_reader_service` | FROZEN / IMPLEMENTED |
| `ManagerModule.projection_service` | FROZEN / IMPLEMENTED |
| `ManagerModule.draft_validation_service` | FROZEN / IMPLEMENTED |
| `source_history_service` opcional | FROZEN / IMPLEMENTED |
| `workflow_service` legacy | SUPERSEDED |
| campos `exact_source_*` | SUPERSEDED |
| `exact_projection_service` | SUPERSEDED |
| `ExactSourceReaderWorkflow` | SUPERSEDED |
| `ExactSourcePublicationWorkflow` | SUPERSEDED |
| `ExactSourceHistoryWorkflow` | SUPERSEDED |
| `ExactProjectionWorkflow` como frontera Manager | SUPERSEDED |
| doble routing exact/legacy | FORBIDDEN |
| adapters/shims/aliases para conservar contrato anterior | FORBIDDEN |

## Manager Source invariants

| Decisión | Estado |
|---|---|
| Source BASE = `SourceSnapshot` | FROZEN |
| Publication recibe `expected_source_snapshot` | FROZEN |
| conflicto se determina por release identity | FROZEN |
| cambio aislado de concurrency token no implica nueva release | FROZEN |
| History usa `HistoryPage` + `SourceReleaseRef` | FROZEN |
| History read devuelve la release solicitada | FROZEN |
| Source identity no se reduce a revision string | FROZEN |

## Manager Projection invariants

| Decisión | Estado |
|---|---|
| `ProjectionTarget` llega completo a `project(...)` | FROZEN |
| Manager no reconstruye target desde revision | FROZEN |
| `ProjectionExecutionResult.target` conserva target ejecutado | FROZEN |
| target con `source_key` distinto al módulo es inválido | FROZEN |
| no existe Manager Projection model legacy paralelo | FROZEN |

## Manager Workspace

| Decisión | Estado |
|---|---|
| workspace conserva `SourceSnapshot` | FROZEN |
| local revision identifica payload local | FROZEN |
| local revision != Source release identity | FROZEN |
| parser de schema anterior no se adapta | FROZEN CLEAN CUTOVER |
| historical load conserva current BASE | FROZEN |
| publicar después crea nueva Source release | FROZEN |

## Navigation

| Decisión | Estado |
|---|---|
| Navigation consume Manager genérico directamente | FROZEN / IMPLEMENTED |
| Navigation no tiene arquitectura Manager especial | FROZEN |
| legacy adapters/configuration stores | SUPERSEDED / REMOVED |
| `expected_source_revision` | SUPERSEDED / REMOVED |
| compatibility shims/aliases | FORBIDDEN |

## Users

| Decisión | Estado |
|---|---|
| Users Manager consume contrato genérico Manager | IMPLEMENTED / VERIFIED / CURRENT |
| `UsersProfilesConfiguration` es aggregate CURRENT | CURRENT |
| `UsersConfigurationCatalog` como authoring paralelo | SUPERSEDED / REMOVED |
| `UserProfileConfiguration` paralelo | SUPERSEDED / REMOVED |
| `expected_source_revision` | SUPERSEDED / REMOVED |
| `projection_source_revision` | SUPERSEDED / REMOVED |
| schema v1 reader dentro de runtime | SUPERSEDED / REMOVED |
| `decode_users_profiles_schema_v1(...)` | SUPERSEDED / REMOVED |
| adapters permanentes para historia durable | FORBIDDEN |
| migración histórica, si existe necesidad real | EXPLICIT ONE-OFF OPERATION ONLY |
| `USERS-CLEAN-CUTOVER-COMPLETION` | CLOSED / VERIFIED / CURRENT |
| `USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL` | CLOSED / VERIFIED / CURRENT |

## Decisiones reemplazadas o refinadas

1. `USERS-MANAGER-ALIGNMENT-VALIDATION` era sólo análisis.
   → **REFINED**: Users Manager fue alineado al contrato genérico.

2. Conservar schema-v1 read compatibility para historia durable.
   → **SUPERSEDED / REMOVED**.

3. Declarar Users CLOSED sólo por suite GREEN.
   → **SUPERSEDED**.

4. Preservar lectura vieja porque existían tests.
   → **SUPERSEDED**.

5. Usar qualification para decidir qué legacy conservar.
   → **REFINED**: primero clean cutover; luego qualification del estado final.

6. Abrir Tools/KPI antes de cerrar Users.
   → **SUPERSEDED**. El prerequisito Users ya está satisfecho.

## Qualification observada

| Hallazgo | Estado |
|---|---|
| checkpoint publicado | VERIFIED / `a065f45c55a527c96ce333705465487e95f0a737` |
| Users scoped Ruff | VERIFIED / PASS |
| Users scoped tests | VERIFIED / 99 passed |
| Full Web | VERIFIED / 545 passed, 7 skipped |
| `git diff --check HEAD^..HEAD` | VERIFIED / PASS |
| working tree final | VERIFIED / CLEAN |
| forbidden scan sobre código CURRENT | VERIFIED / zero matches para lista inspeccionada |
| schema-v1 compatibility residue | VERIFIED / REMOVED |
| Tools consumer | UNVERIFIED / PLANNED NEXT |
| KPI Configuration consumer | UNVERIFIED / PLANNED |
| KPI Definition consumer | UNVERIFIED / PLANNED |
| Docker E2E | UNVERIFIED |
| Python 3.14.7 global | UNVERIFIED |

## Siguiente foco único

```text
TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED / NEXT
```

No inferir que Tools necesita el mismo cutover que Users.
