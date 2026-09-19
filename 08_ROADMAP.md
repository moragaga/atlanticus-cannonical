# Atlanticus — Roadmap

Estado: **CANONICAL BASELINE 1.0 — EXECUTION IN PROGRESS**

## Regla

Un solo foco por incremento.

Cerrar cada frontera con evidencia suficiente para su alcance.

No conservar legacy para sostener consumers o tests anteriores.

No mezclar cleanup transversal con el incremento funcional activo.

## Checkpoint publicado de referencia

```text
moragaga/atlanticus@fbef06a8a0a587571527d9ecf131c73c5fc5f01a
```

Parent:

```text
9f12c41a23d69784c7c5b775a4093a94ac654d55
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

CONFIGURATION-UI-COMPOSITION-RECOVERY
CLOSED / VERIFIED / CURRENT

GENERIC-WEB-PAGINATION-CUTOVER
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

ada.web.configuration.pagination
SUPERSEDED / REMOVED

ConfigurationPageRequest / ConfigurationPage generic names
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

## Paginación generic CURRENT

```text
atlanticus.web.pagination
CURRENT

page sizes
10 | 20

presentation ownership
LOCAL TO EACH UI
```

No mover markup/CSS/placeholder logic a Atlanticus sólo para uniformar apariencia.

## Secuencia siguiente

```text
1. PROFILES-CONFIGURATION-EDITOR-CONTRACT
   PLANNED / NEXT

2. PROFILES-CONFIGURATION-WEB-SURFACE
   PLANNED

3. USERS-ADMINISTRATION-UI
   PLANNED / SEPARATE

4. ADA-ACCESS-CONFIGURATION-UI
   PLANNED / SEPARATE

5. MANAGER-FINAL-ADMIN-COMPOSITION
   PLANNED / FINAL
```

No implementar las superficies faltantes en un solo incremento.

No crear un framework UI transversal nuevo sin evidencia de reutilización real.

## Frentes separados que permanecen abiertos

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT

exact guest fallback composition for authenticated non-promoted identities
PLANNED / SEPARATE

ADA Access runtime composition
PLANNED / SEPARATE

concrete Entra/Graph UsersDirectoryReader provider
UNVERIFIED

WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN

PYTHON-METADATA-ALIGNMENT
PLANNED / OPEN

CI remote fbef06a8...
UNVERIFIED

full Ruff workspace
UNVERIFIED
```
