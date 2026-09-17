# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global

| Decisión | Estado |
|---|---|
| Python 3.14.7 | DECIDED / NOT YET QUALIFIED GLOBALLY |
| `python:3.14.7-slim-trixie` | DECIDED / NOT YET QUALIFIED GLOBALLY |
| `uv`, no pip normal | CURRENT |
| Definir contratos antes que consumidores | CURRENT |
| Backend antes que frontend | CURRENT |
| Cutover raíz limpio | CURRENT |
| No crear shims/adapters/aliases temporales para legacy | FROZEN |
| No conservar doble contrato | FROZEN |
| Tests no son autoridad sobre contratos SUPERSEDED | FROZEN |
| Un consumer puede quedar temporalmente roto durante un root cutover | FROZEN |

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
Generic Atlanticus infrastructure contract where applicable
```

Usar infraestructura genérica no cambia automáticamente ownership de dominio.

## Source / Projection

| Decisión | Estado |
|---|---|
| Source genérico pertenece a `web/capabilities/source` | CURRENT |
| Projection exact-release pertenece a `web/capabilities/projection/core` | CURRENT |
| Release identity != content hash | FROZEN |
| Source current nunca lo determina Cosmos | FROZEN |
| Projection target = `SourceKey + SourceReleaseRef + dependencies` | FROZEN |
| Projection dependencies son exact `ProjectionTarget` | FROZEN / IMPLEMENTED |
| `project(target)` no relee current | FROZEN |
| CURRENT/OUTDATED compara exact target | FROZEN |
| No reconstruir `ProjectionTarget` desde revision | FROZEN |
| `expected_source_revision` | SUPERSEDED / REMOVED |
| private projection revision identity | SUPERSEDED / REMOVED |

## Manager generic contract

| Decisión | Estado |
|---|---|
| Manager tiene un solo contrato Source/Projection genérico | FROZEN / IMPLEMENTED |
| `ManagerModule.source_key` | FROZEN / IMPLEMENTED |
| `ManagerModule.source_service` | FROZEN / IMPLEMENTED |
| `ManagerModule.source_reader_service` | FROZEN / IMPLEMENTED |
| `ManagerModule.projection_service` | FROZEN / IMPLEMENTED |
| `ManagerModule.draft_validation_service` | FROZEN / IMPLEMENTED |
| `source_history_service` opcional | FROZEN / IMPLEMENTED |
| `workflow_service` legacy | SUPERSEDED / REMOVED |
| campos `exact_source_*` | SUPERSEDED / REMOVED |
| `exact_projection_service` | SUPERSEDED / REMOVED |
| doble routing exact/legacy | FORBIDDEN |
| adapters/shims/aliases para conservar contrato anterior | FORBIDDEN |

## Manager invariants

```text
Source BASE = SourceSnapshot
Workspace identity != Source identity
ProjectionTarget llega completo a project(...)
Manager no reconstruye Source/Projection identity desde revision strings
History usa HistoryPage + SourceReleaseRef
```

Estado: **FROZEN / CURRENT**.

## Semántica estructural de capabilities

La misma responsabilidad debe usar el mismo concepto y naming.

Patrón vigente cuando existan ambas responsabilidades:

```text
<capability>/
├── core
└── configuration
```

Semántica:

```text
core
domain models / invariants / contracts propios

configuration
editable/publishable configuration contract y lifecycle asociado cuando exista
```

No crear una arquitectura especial para una capability sin una frontera técnica
o funcional real.

Para Profiles:

```text
profiles/core
CURRENT

profiles/configuration
CURRENT
```

La propuesta `profiles/management` queda `SUPERSEDED / NOT ADOPTED`.

`management` no se usa como sinónimo genérico de configuration. Cuando exista
como concepto de dominio, conserva ese significado específico.

## Users / Profiles / Navigation

Target de composición:

```text
Users
  │ standalone válido
  ▼
Profiles
  ▼
Navigation
```

Estados contractuales:

```text
Users standalone
REQUIRED

Profiles without Users
INVALID COMPOSITION

Navigation without Profiles
INVALID COMPOSITION

Users + Navigation without Profiles
INVALID COMPOSITION
```

Esto no obliga a acoplar innecesariamente los cores.

Navigation debe preferir una autoridad/access key efectiva inyectada por
composition antes que importar modelos concretos de Profiles.

## Users base authority

Contrato congelado:

```text
guest
TRANSITIONAL / NON-ASSIGNABLE

basic
ASSIGNABLE / STANDARD

root
ASSIGNABLE / FULL AUTHORITY

local
LOCAL-RUNTIME ONLY / NON-ASSIGNABLE / FULL AUTHORITY
```

`administrator` queda `SUPERSEDED / REMOVE`.

No crear alias, shim o mapping:

```text
administrator -> root
```

Jane Doe y John Doe son identidades locales, no perfiles.

Colores congelados:

```text
Jane Doe
#C85D91 / #FFFFFF

John Doe
#3778C2 / #FFFFFF

guest
#FF5722 / #FFFFFF
```

## Profiles ownership

Profiles es first-class capability.

CURRENT parcial:

```text
profiles/core
profiles/configuration
```

Target pendiente:

```text
Profiles own Source
Profiles own Projection/configuration lifecycle
Profiles own administration
Profiles own UI
```

No conservar ownership combinado Users/Profiles al completar el cutover.

## Users / Profiles combined contracts

Target:

```text
UsersProfilesConfiguration
REMOVE

UsersProfilesAdministrationService
REMOVE

UsersProfilesAdminDraft
REMOVE

Profiles resource inside Users Source
REMOVE

Profiles UI inside Users UI
REMOVE
```

No recrear atomicidad con una transacción distribuida.

Cada capability publica su propio Source.

## Testing

Tests protegen:

```text
behavior
contracts
invariants
regressions
critical flows
```

No crear tests cuyo único objetivo sea:

```text
CSS visual
spacing
responsive
branding
apariencia
estructura visual
existencia/no existencia de funciones o clases
source-token scans
import scans
AST/module structure
detalles internos de implementación
```

Assets JS/CSS pueden comprobarse sólo cuando su existencia/carga sea
contractualmente relevante.

La validación visual es legítima para apariencia y responsive.

Un test CURRENT que inspecciona source para probar ausencia de Profiles en
Users core permanece OPEN y debe tratarse bajo `WEB-TEST-CONTRACT-CLEANUP`, no
usarse como precedente.

## Estado de ejecución

```text
USERS-STANDALONE-AUTHORITY-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-BOUNDARY-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CAPABILITY-EXTRACTION
IN PROGRESS

USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER
PLANNED / NEXT
```

## Decisiones reemplazadas o refinadas

1. Profiles bajo un package genérico `management`.
   → **SUPERSEDED / NOT ADOPTED**.

2. `ProfilesConfiguration` dentro de `profiles/core`.
   → **SUPERSEDED / REMOVED**; owner CURRENT `profiles/configuration`.

3. Navigation standalone o profile binding opcional dentro del target Atlanticus.
   → **SUPERSEDED**; composition target exige Navigation => Profiles => Users.

4. Tests como mecanismo para congelar ausencia de imports/files/funciones.
   → **FORBIDDEN**; validar comportamiento/contratos.

5. Tests automatizados de apariencia CSS.
   → **FORBIDDEN AS CONTRACT TESTS**; apariencia se valida visualmente.

## Qualification del checkpoint

Implementación CURRENT:

```text
moragaga/atlanticus@4e008055ddc551e6c08a7d87715340c8c7cd149e
```

Parent:

```text
709cf2fb9ee422094f011cfda051f08f37276992
```

Evidencia observada:

```text
Users standalone affected suites
92 PASS

Profiles configuration boundary affected suites
74 PASS

git diff --check
PASS in both cutovers
```

No trasladar esos resultados a suites o flujos no ejecutados.

## Conflicto abierto de Python metadata

Decisión global:

```text
Python 3.14.7
```

Packages CURRENT todavía declaran en varios puntos:

```text
requires-python = "==3.14.2"
```

Permanece OPEN y fuera del siguiente incremento.

## Siguiente foco único

```text
USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER
PLANNED / NEXT
```
