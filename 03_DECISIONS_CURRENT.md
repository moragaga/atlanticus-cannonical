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

Una pieza no deja de ser compatibilidad por ser:

- read-only;
- privada;
- interna al codec;
- usada para historia durable;
- necesaria para mantener tests existentes.

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

Los tests se corrigen cuando validan un contrato SUPERSEDED.

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
| `expected_source_revision` | SUPERSEDED / REMOVE |

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
| Users Manager consume contrato genérico Manager | IMPLEMENTED / VERIFIED |
| `UsersProfilesConfiguration` es aggregate CURRENT | CURRENT |
| `UsersConfigurationCatalog` como authoring paralelo | SUPERSEDED / REMOVE |
| `UserProfileConfiguration` paralelo | SUPERSEDED / REMOVE |
| `expected_source_revision` | SUPERSEDED / REMOVE |
| `projection_source_revision` | SUPERSEDED / REMOVE |
| schema v1 reader dentro de runtime | SUPERSEDED / REMOVE |
| `decode_users_profiles_schema_v1(...)` | SUPERSEDED / REMOVE |
| adapters permanentes para historia durable | FORBIDDEN |
| migración histórica, si existe necesidad real | EXPLICIT ONE-OFF OPERATION ONLY |

## Decisions superseded/refined durante este cierre

1. `USERS-MANAGER-ALIGNMENT-VALIDATION` era sólo análisis.
   → **REFINED**: el Manager consumer de Users fue alineado al contrato genérico.

2. Se propuso conservar schema-v1 read compatibility para historia durable.
   → **SUPERSEDED**: viola el clean cutover. Debe eliminarse completamente del runtime CURRENT.

3. Se consideró Users CLOSED cuando Ruff/pytest y scan nominal estaban verdes.
   → **SUPERSEDED**: GREEN de tests no sustituye cumplimiento del contrato congelado.

4. Se usaron tests de schema viejo como razón para preservar lectura.
   → **SUPERSEDED**: tests que sólo defienden legacy se eliminan o reescriben después del cutover.

5. La qualification podía guiar qué legacy conservar.
   → **REFINED**: primero se completa la migración; después qualification detecta desalineaciones del estado final.

6. Tools/KPI podía abrirse inmediatamente tras suite Web GREEN.
   → **REFINED**: primero cerrar `USERS-CLEAN-CUTOVER-COMPLETION`.

## Qualification observada

| Hallazgo | Estado |
|---|---|
| baseline publicado inspeccionado | VERIFIED / `55cd6121e000a6af5d4f0dc0ea2e384f97a27f2a` |
| Users scoped Ruff local | VERIFIED / PASS |
| Users scoped tests local | VERIFIED / 113 passed |
| Full Web local | VERIFIED / 546 passed, 7 skipped |
| `git diff --check` local | VERIFIED / PASS |
| legacy-name exact scan ejecutado | VERIFIED / 0 matches para lista inspeccionada |
| schema-v1 compatibility residue | VERIFIED / PRESENT / MUST REMOVE |
| Tools consumer | UNVERIFIED |
| KPI Configuration consumer | UNVERIFIED |
| KPI Definition consumer | UNVERIFIED |
| Docker E2E | UNVERIFIED |
| Python 3.14.7 global | UNVERIFIED |

## Siguiente foco único

```text
USERS-CLEAN-CUTOVER-COMPLETION
PLANNED / NEXT
```

No abrir ningún otro consumer hasta cerrarlo.
