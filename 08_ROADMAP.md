# Atlanticus — Roadmap

Estado: **CANONICAL BASELINE 1.0 — EXECUTION IN PROGRESS**

## Regla

Un solo foco por incremento.

Cerrar cada frontera con evidencia suficiente para su alcance.

No conservar legacy para sostener consumers o tests anteriores.

No mezclar cleanup transversal con el incremento funcional activo.

## Checkpoint publicado de referencia

```text
moragaga/atlanticus@3eb46dac80f23d438774e3afa39999dc96f592d7
```

Parent:

```text
0fba548329afd9bc9dee92ea6caa53d1aaa69eb0
```

## Hitos cerrados relevantes

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

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

USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-PERSISTED-DATA-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-BOUNDARY-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CAPABILITY-EXTRACTION
CLOSED / VERIFIED / CURRENT

PROFILES-INDEPENDENT-SOURCE-LIFECYCLE
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROFILES-CONFIGURATION
CLOSED / VERIFIED / CURRENT

NONPROMOTED-ACCESS-SEMANTICS-CORRECTION
CLOSED / VERIFIED / CURRENT

NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
CLOSED / VERIFIED / CURRENT
```

## Hitos superados

```text
USERS-STANDALONE-AUTHORITY-CUTOVER
CLOSED / SUPERSEDED BY USERS-GLOBAL-REGISTRY-ROOT-CUTOVER

USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER
SUPERSEDED / NOT FINAL TARGET

USERS-PROFILES-COMPOSITION-CUTOVER
SUPERSEDED AS PREVIOUS MODEL

ACCESS-PROFILES-CONFIGURATION as generic Atlanticus Access target
SUPERSEDED / NOT ADOPTED

Navigation local profile mini-model
SUPERSEDED / REMOVED
```

## Navigation / Profiles CURRENT

```text
Navigation Configuration -> Profiles core
CURRENT

Navigation -> Users
FORBIDDEN

Navigation -> ADA Access
FORBIDDEN

Navigation -> Profiles Configuration
FORBIDDEN

Navigation durable profile references
allowed_profiles = profile keys
```

No existe catálogo local `_BASE_PROFILES` ni adapter equivalente.

## Siguiente foco único recomendado

```text
Manager authorization stale administrator/local semantics
PLANNED / PROPOSED NEXT
```

Primera etapa obligatoria:

```text
inspect ManagerPrincipal contract
inspect ManagerModuleAccess contract
inspect DefaultManagerAuthorizationPolicy behavior
inspect ADA Configuration Manager duplicated _can_manage_* helpers
identify exact local-development composition requirements
freeze final authorization semantics before implementation
```

No asumir que `local`, `root`, Profiles o ADA Access deban mapearse automáticamente a
Manager permissions.

No conservar compatibility con `administrator` si el contrato final lo elimina.

## Frentes separados que permanecen abiertos

```text
exact guest fallback composition for authenticated non-promoted identities
OPEN / SEPARATE

USERS-ADMINISTRATION-SURFACE-CUTOVER
PLANNED / SEPARATE

ADA Access runtime composition
OPEN / SEPARATE

concrete Entra/Graph UsersDirectoryReader provider
UNVERIFIED

WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN

PYTHON-METADATA-ALIGNMENT
PLANNED / OPEN

CI remote
UNVERIFIED
```

## No mezclar en el siguiente chat

- Navigation Configuration redesign;
- Navigation guest fallback runtime composition;
- Users Administration UI/repair;
- ADA Access runtime composition;
- Python metadata cleanup;
- Web test cleanup global;
- unrelated Ruff cleanup;
- Command Center;
- Operational Data;
- rediseño de Source/Projection core.

Único foco recomendado:

```text
Manager authorization stale administrator/local semantics
```
