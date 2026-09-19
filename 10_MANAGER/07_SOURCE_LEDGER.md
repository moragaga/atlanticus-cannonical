# Manager — Source Ledger

Estado: **AUDIT LEDGER**

## Autoridad

- `moragaga/atlanticus:main` = realidad implementada publicada.
- `moragaga/atlanticus-cannonical:main` = autoridad documental vigente.
- `moragaga/atlanticus-decisions` = HISTORICAL.
- Git permanece SOLO LECTURA para el asistente.

## Checkpoint publicado de este cierre

```text
moragaga/atlanticus@415c8263c15bae2b5d3c01b734b0f1e0101a7242
```

Parent:

```text
9b623d67e253413f6b0d894e10d9cc15735b553d
```

Tree:

```text
ff81bcd74b5e844604ca656562f9aaf398946d67
```

## Cierres anteriores relevantes ya CURRENT

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-MANAGER-COMPOSITION
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROJECTION-PERSISTENCE
CLOSED / VERIFIED / CURRENT
```

## Profiles -> ADA Configuration Manager

Estado:

```text
PROFILES-ADA-CONFIGURATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT
```

Implementado:

```text
compose_profiles_manager(...)
→ produce ManagerModule existente
→ ManagerModule.web_module registra Source/Projection/Validation
→ registro ocurre sobre el ServiceRegistry real de Atlanticus Web

ADA Configuration Manager
→ recibe profiles_module
→ lo incluye en ManagerSurfaceDefinition
```

No implementado:

```text
registry temporal
adapter de integración
shim
alias
segundo contrato Manager para Profiles
```

Capability funcional:

```text
profiles.manage
```

Local runtime CURRENT:

```text
profiles.manage
navigation.manage
tools.manage
kpis.manage
```

## Qualification observada

Durante este cierre:

```text
profiles-manager pytest             8 PASS
ADA Configuration Manager pytest   26 PASS
Ruff sobre archivos del incremento  PASS
git diff --check                    PASS
```

El commit publicado fue verificado remotamente.

No declarar:

```text
full Web pytest GREEN
full ADA pytest GREEN
full Ruff workspace GREEN
CI remote GREEN
Storage/Cosmos E2E GREEN
Python 3.14.7 global qualification
```

salvo evidencia posterior.

## ADA Access Projection persistence

Checkpoint parent:

```text
9b623d67e253413f6b0d894e10d9cc15735b553d
```

Estado:

```text
ADA-ACCESS-PROJECTION-PERSISTENCE
CLOSED / VERIFIED / CURRENT
```

CURRENT incluye:

```text
durable ProjectionRecord[AdaAccessConfiguration] serialization
exact recursive ProjectionTarget dependencies
local projection provider
Cosmos projection provider
standalone storage topology
```

No reconstruir provenance desde CURRENT Profiles después de restart.

## UI CURRENT observada

ADA Configuration Manager compone:

```text
profiles
navigation
tools
kpis
kpi-definitions
```

Todavía no compone:

```text
Users Administration
ADA Access Configuration
```

La ausencia es de superficie/composition administrativa.
No autoriza reconstruir dominios ni inventar contracts paralelos.

## Finding pendiente separado

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

`web/compositions/navigation-manager` usa un consumer desalineado respecto de
`ManagerAuthorizationPolicy.can_view(...)`.

No introducir alias.

## Próxima frontera

```text
USERS-ADMINISTRATION-MANAGER-INTEGRATION
PLANNED / NEXT / DESIGN FIRST
```

CURRENT Users ya tiene:

```text
UsersAdministrationService
```

y no tiene:

```text
users-manager composition
Users Manager Source/Projection module
```

La etapa siguiente debe seguir ese lifecycle existente.

## Después

```text
ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
PLANNED

MANAGER-FINAL-ADMIN-COMPOSITION
PLANNED
```
