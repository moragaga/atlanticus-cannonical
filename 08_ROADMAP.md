# Atlanticus — Roadmap

Estado: **CANONICAL BASELINE 1.0 — EXECUTION IN PROGRESS**

## Regla

Un solo foco por incremento.

Cerrar cada frontera con evidencia suficiente para su alcance.

No conservar legacy para sostener consumers o tests anteriores.

No mezclar cleanup transversal con el incremento funcional activo.

## Checkpoint publicado de referencia

```text
moragaga/atlanticus@9f12c41a23d69784c7c5b775a4093a94ac654d55
```

Parent:

```text
3eb46dac80f23d438774e3afa39999dc96f592d7
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

MANAGER-AUTHORIZATION-SEMANTICS-ALIGNMENT
CLOSED / VERIFIED / CURRENT

MANAGER-ACTIVE-WORKFLOW-CALLBACK-CARDINALITY
CLOSED / VERIFIED / CURRENT
```

## Hitos superados

```text
ManagerModuleAccess
SUPERSEDED / REMOVED

per-operation Manager validate/publish/project access fields
SUPERSEDED / REMOVED

is_local Manager authorization bypass
SUPERSEDED / REMOVED

administrator profile Manager authorization bypass
SUPERSEDED / REMOVED

Navigation local profile mini-model
SUPERSEDED / REMOVED

Users Source/Projection Manager model
SUPERSEDED / REMOVED
```

## Finding CURRENT no cerrado

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

No resolver con alias/shim `can_access`.

## UI administrativa CURRENT

Presentes:

```text
Navigation
Tools
KPI Configuration
KPI Definition
```

Faltantes:

```text
Profiles Configuration UI
Users Administration UI
ADA Access Configuration UI
```

Backend/lógica existente:

```text
Profiles Configuration
→ ProfilesConfiguration + Profiles Source lifecycle

Users Administration
→ UsersAdministrationService + stores/contracts actuales

ADA Access Configuration
→ AdaAccessConfiguration + ADA Access Source lifecycle
```

No diseñar nuevos dominios para crear esas superficies.

## Siguiente foco único recomendado

```text
CONFIGURATION-UI-COMPOSITION-RECOVERY
PLANNED / NEXT
```

Primera etapa obligatoria:

```text
inspect CURRENT admin composition and Manager shell
inventory reusable UI/composition primitives already implemented
locate historical transversal UI/composition evidence when available
identify visualizations that are actually missing in CURRENT
expose current conflicts instead of adding compatibility
freeze one reusable composition boundary only if evidence requires it
choose one first missing UI increment
```

No implementar las tres UI faltantes en un solo incremento.

No crear un framework transversal nuevo sin evidencia de reutilización o código previo.

## Frentes separados que permanecen abiertos

```text
exact guest fallback composition for authenticated non-promoted identities
OPEN / SEPARATE

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
