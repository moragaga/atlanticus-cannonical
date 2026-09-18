# Atlanticus — Open Questions

Estado: **CANONICAL OPEN ITEMS**

Los puntos aquí no reabren contratos CLOSED.

## CLOSED — Manager generic core

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

No están OPEN:

```text
double routing exact/legacy
workflow_service lifecycle
ExactProjectionWorkflow
expected_source_revision
revision -> ProjectionTarget reconstruction
compatibility shims/adapters
```

## CLOSED — Configuration domains

```text
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT

TOOLS-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## CLOSED — Users global registry root cutover

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Ya no están OPEN:

```text
Users as Manager module
Users Source workflow
Users generic Projection
UsersProfilesConfiguration
UsersProfilesAdministrationService
UsersProfilesAdminDraft
Cosmos pending/resolved runtime contract
login observe/write pending
Users configuration package
Users projection-cosmos package
users-manager composition
```

## OPEN — Users persisted data cutover

```text
USERS-PERSISTED-DATA-CUTOVER
PLANNED / NEXT
```

Motivo:

El código CURRENT rechaza contratos legacy, pero este cierre no inspeccionó ni
migró los datos persistidos reales.

Preguntas obligatorias del siguiente chat:

1. ¿Qué datos Users/Profiles existen realmente en los stores/deployments actuales?
2. ¿Qué documentos Cosmos pertenecen al schema legacy `pending` / `resolved` y cuáles al schema CURRENT?
3. ¿Existe ya un Blob registry compatible con `atlanticus_users_registry` schema 1?
4. ¿Qué información de los antiguos payloads pertenece a Users globales y cuál debe preservarse para Profiles?
5. ¿Cuál es la topología/configuración real de container/blob/connections sin inventar nombres ni credenciales?
6. ¿Qué operación one-shot permite migrar y verificar sin introducir lectores legacy runtime?
7. ¿Qué condición exacta permite borrar los datos legacy después de verificar parity?

No asumir que un dato existe o está vacío sin inspección.

## OPEN — Users Administration surface

```text
USERS-ADMINISTRATION-SURFACE-CUTOVER
PLANNED / AFTER PERSISTED DATA
```

El core ya expone:

```text
PROMOTABLE
CONFLICT
PROMOTED
```

pero este hito no implementó UI ni repair commands.

No reintroducir Users en Configuration Manager como Source/Projection.

## OPEN — concrete Entra directory discovery

Contrato disponible:

```text
UsersDirectoryReader
```

Provider concreto Graph/Entra:

```text
UNVERIFIED
```

No inventar tenant settings, Graph permissions, credential flow ni endpoints.

## OPEN — Profiles lifecycle

```text
PROFILES-CAPABILITY-EXTRACTION
IN PROGRESS

PROFILES-INDEPENDENT-SOURCE-LIFECYCLE
PLANNED
```

`profiles/core` y `profiles/configuration` existen.

No está cerrado todavía:

```text
independent Source/Projection lifecycle
administration surface
app-specific User/Profile association
Access configuration ownership
```

No reabrir Users registry para resolver estos puntos.

## OPEN — Navigation / Access integration

La anterior cadena rígida:

```text
Users Source -> Profiles Source -> Navigation
```

queda SUPERSEDED porque Users ya no es Source.

Permanece OPEN cómo Navigation consume el resultado efectivo de Profiles/Access sin
adquirir ownership de Users.

## OPEN — Python package metadata alignment

Canonical fija:

```text
Python 3.14.7
```

CURRENT remoto:

```text
web/pyproject.toml
requires-python = "==3.14.2"
```

La qualification local del cutover usó Python 3.14.7, pero la metadata continúa
inconsistente.

Estado:

```text
PLANNED / OPEN
```

## PLANNED — Web test contract cleanup

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED
```

No abrir este frente durante persisted-data cutover salvo que un test directamente
afectado contradiga el comportamiento final legítimo.

## UNVERIFIED

- production Blob registry presence/content;
- production Cosmos legacy/current record inventory;
- final deletion conditions for persisted legacy data;
- concrete Entra/Graph directory provider;
- full Ruff workspace after current commit;
- full ADA regression;
- CI remoto;
- Python metadata/Trixie global qualification;
- exact app-specific User → Profile/Access association contract.

## Siguiente foco

```text
USERS-PERSISTED-DATA-CUTOVER
```

Fuentes obligatorias:

```text
moragaga/atlanticus:main
moragaga/atlanticus-cannonical:main
```

`moragaga/atlanticus-decisions` es sólo HISTORICAL.

Git sólo lectura para el asistente.
