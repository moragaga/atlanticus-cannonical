# Web Platform — Source Ledger

Estado: **AUDIT LEDGER**

## Corte CURRENT

```text
moragaga/atlanticus@29bbf6d8f2b47a7d31e967ad4bb8de42f67a4c85
```

Parent:

```text
856498c52f182cd531deae845c25bd51ae2ff4ea
```

Tree:

```text
3f27ad599c6dec610dff5317494a73b276d2ebc4
```

## Users

CURRENT ownership:

```text
identity
lifecycle
user -> profile_key
```

No existe CURRENT:

```text
authority_key
users/configuration
Users generic Projection
Users Manager Source/Projection module
```

## Profiles

CURRENT:

```text
profiles/core
profiles/configuration
profiles/projection-local
profiles/projection-cosmos
web/compositions/profiles-manager
```

Profiles es generic Atlanticus y posee `ProfileCatalog`.

## ADA Access

CURRENT:

```text
profile_key -> access_keys
```

ADA Access Configuration Web/Manager ya está implementado.

El estado histórico que indicaba ausencia de Web surface queda SUPERSEDED.

## Navigation

CURRENT boundary:

```text
Navigation Configuration -> Profiles core
REMOVED

Navigation Configuration -> Users
FORBIDDEN

Navigation Configuration -> ADA
FORBIDDEN

Navigation -> Users
FORBIDDEN

Navigation -> ADA Access
FORBIDDEN
```

Neutral integration contract:

```text
NavigationProfileOption
NavigationProfileOptionsProvider
```

`allowed_profiles` persiste profile keys como strings.

Semántica:

```text
()
PUBLIC WITHIN NAVIGATION AUTHORIZATION

non-empty
RESTRICTED
```

ADA composition adapta `ProfileCatalog` y excluye `root`/`local` del selector asignable.

## Navigation Configuration Web

CURRENT:

```text
profiles context card
REMOVED

profile assignment
LINK EDITOR ONLY

guest auto-selection
REMOVED

pagination
TOP-LEVEL / 10-20

section expansion
ALL CHILDREN / EPHEMERAL

empty state
RESERVED HEIGHT + CENTERED

horizontal overflow
FIXED AT DASH FOCUS TARGET CONTAINMENT
```

## Manager

CURRENT distingue:

```text
ManagerModule
ManagerEntry
```

ADA Configuration Manager compone:

```text
Administration:
- Users

Configuration:
- Profiles
- Access
- Navigation
- Tools
- KPI
- KPI Definition
```

## Qualification observada

Antes de la patch visual final:

```text
22 Navigation core passed
42 Navigation Configuration passed
10 Navigation Manager passed
5 ADA Configuration Manager focused passed
```

Post-current-checkpoint qualification:

```text
UNVERIFIED
```

## Finding pendiente

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

No añadir alias.

## UI review

```text
MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS
```

Siguiente página:

```text
Herramienta
```

Después de cerrar apariencia desktop de las páginas, auditar media queries; al final ejecutar
cleanup/qualification de tests.
