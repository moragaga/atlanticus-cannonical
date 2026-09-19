# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global

| Decisión | Estado |
|---|---|
| Python 3.14.7 | DECIDED / LOCALLY USED / METADATA NOT YET GLOBALLY ALIGNED |
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
Responsabilidad real del dominio; infraestructura genérica sólo donde aplique
```

Usar infraestructura genérica no cambia automáticamente ownership de dominio.

No forzar Source/Projection/Manager sobre una entidad que no sea configuration lifecycle.

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

La autorización interna de Manager todavía tiene semántica stale de `is_local` y
`administrator`; no se redefine silenciosamente en este documento.

## Global Users

Users es registry/lifecycle de entidad global y no configuration Source.

```text
Users != Configuration Source
Users != Profile assignment
Users != Access configuration
Users != Navigation configuration
```

Strong identity:

```text
(issuer, subject_id)
user_id = build_user_key(issuer, subject_id)
```

Managed global authority:

```text
basic
root
```

Runtime local authority:

```text
local
```

No existen como Users authority CURRENT:

```text
guest
administrator
administrator -> root
```

Runtime login permanece read-only.

CURRENT:

```text
record absent
→ READY
→ deterministic user_id
→ no UsersRuntime EffectiveUser

record present + enabled=True
→ READY
→ EffectiveUser available

record present + enabled=False
→ USER_DISABLED
→ 403
```

`USER_NOT_PROMOTED` queda SUPERSEDED / REMOVED.

## Profiles

Profiles es capability generic Atlanticus first-class.

Ownership CURRENT:

```text
profiles/core
ProfileDefinition
ProfileCatalog
profile domain invariants

profiles/configuration
ProfilesConfiguration
Profiles Source lifecycle
```

`ProfileDefinition`:

```text
key
label
background_color
text_color
```

No agregar permisos ADA ni options application-specific al modelo generic.

## ADA Access

Generic Atlanticus Access no fue adoptado.

ADA Access es application-specific y permanece bajo ADA.

```text
Global user_id -> ADA profile_keys
ADA profile_key -> ADA access_keys
```

ADA Access puede consumir `ProfileCatalog` para validar referencias, pero Navigation
no depende de ADA Access.

## Navigation / Profiles alignment

Estado:

```text
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
CLOSED / VERIFIED / CURRENT
```

### Dependency boundary

```text
Navigation Configuration -> Profiles core
CURRENT

Navigation Configuration -> Profiles Configuration
FORBIDDEN

Navigation -> Users
FORBIDDEN

Navigation -> ADA Access
FORBIDDEN
```

### Durable contract

```text
NavigationLinkConfiguration.allowed_profiles
= tuple de profile keys
```

Navigation no persiste `ProfileDefinition`.

Navigation core conserva:

```text
principal.unrestricted
OR
principal.access_key in allowed_profiles
```

### Catalog provider

Contrato CURRENT:

```text
NavigationProfileCatalogProvider = Callable[[], ProfileCatalog]
```

Sin provider:

```text
profile_definitions() -> ()
```

Con provider:

```text
profile_definitions(provider) -> provider().all()
```

No existe fallback a perfiles base inventados.

Fallos del provider se propagan.

### Referential validation

`create_navigation_profile_catalog_validator` valida keys mediante
`ProfileCatalog.require`.

Unknown profile:

```text
navigation.profile.unknown
```

El mismo tuple `validators` se utiliza para draft validation y Projection en la
composition Navigation Manager.

### Removed legacy

```text
NavigationProfileOption
NavigationProfileOptionsProvider
_BASE_PROFILES
resolve_profile_options
selectable_profile_options
profile_options_provider
projection_validators parameter name
```

queda:

```text
SUPERSEDED / REMOVED
```

No hay alias ni compatibility layer.

### root / local / guest

`root` y `local` no se modelan como `ProfileDefinition` especiales dentro de Navigation
Configuration.

`NavigationPrincipal.unrestricted` permanece el mecanismo del core para acceso total.

`guest` sólo es un profile normal si existe en el catálogo provisto.

La composición exacta de fallback guest para identidad autenticada no promovida sigue
OPEN / SEPARATE; no crear Users authority `guest` ni UserRecord ficticio.

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

## Qualification del checkpoint

Implementación CURRENT:

```text
moragaga/atlanticus@3eb46dac80f23d438774e3afa39999dc96f592d7
```

Parent:

```text
0fba548329afd9bc9dee92ea6caa53d1aaa69eb0
```

Evidencia observada del incremento Navigation / Profiles:

```text
Python 3.14.7
uv lock --check PASS
focused pytest PASS / 100%
Profiles + Navigation regression PASS / 100%
Ruff increment-owned files PASS
git diff --check PASS
legacy symbol scan 0 matches
```

No atribuir PASS a full Ruff workspace ni CI remoto.

## Conflicto abierto de Python metadata

Decisión global:

```text
Python 3.14.7
```

Navigation Configuration CURRENT todavía declara:

```text
requires-python = "==3.14.2"
```

Permanece OPEN y separado.

## Siguiente foco único recomendado

```text
Manager authorization stale administrator/local semantics
PLANNED / PROPOSED NEXT
```

Antes de implementar debe revisarse el contrato CURRENT de `ManagerPrincipal`,
`ManagerModuleAccess`, `DefaultManagerAuthorizationPolicy` y consumers ADA.
