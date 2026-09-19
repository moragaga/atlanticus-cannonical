# Atlanticus — Roadmap

Estado: **CANONICAL BASELINE 1.0 — EXECUTION IN PROGRESS**

## Regla

Un solo foco por incremento.

Cerrar cada frontera con evidencia suficiente.

No conservar legacy para sostener consumers o tests anteriores.

No mezclar cleanup transversal con el incremento funcional activo.

## Checkpoint publicado de referencia

```text
moragaga/atlanticus@a31fce11d26a7c0a554d82de1813a4311522919b
```

Parent:

```text
90e89c376dfdfd182f0380b1d407127ecb7c9711
```

## Hitos cerrados relevantes

```text
GENERIC-WEB-PAGINATION-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-EDITOR-CONTRACT
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-WEB-SURFACE
CLOSED / VERIFIED / CURRENT

PROFILES-PROJECTION-CONTRACT
CLOSED / VERIFIED / CURRENT

PROFILES-MANAGER-COMPOSITION
CLOSED / VERIFIED / CURRENT

USERS-PROFILES-CONTRACT-REALIGNMENT
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROFILE-OWNERSHIP-REALIGNMENT
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROJECTION-CONTRACT
CLOSED / VERIFIED / CURRENT
```

Los hitos cerrados anteriores de Source/Projection/Manager/Navigation/Tools/KPI permanecen
CURRENT salvo conflicto concreto demostrado.

## Hitos superados

```text
Users authority_key
SUPERSEDED / REMOVED

Users basic|root assignable-authority contract
SUPERSEDED / REMOVED

ADA Access UserProfileAssignment
SUPERSEDED / REMOVED

ADA Access user_id -> profile_keys
SUPERSEDED / REMOVED

Profiles editor/web surface como gap
SUPERSEDED / CLOSED
```

## Finding previo no cerrado

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

No resolver con alias/shim.

## Secuencia siguiente

```text
1. ADA-ACCESS-PROJECTION-PERSISTENCE
   PLANNED / NEXT / DESIGN FIRST

2. ADA-ACCESS-CONFIGURATION-UI
   PLANNED / SEPARATE

3. USERS-ADMINISTRATION-SURFACE-CUTOVER
   PLANNED / SEPARATE

4. MANAGER-FINAL-ADMIN-COMPOSITION
   PLANNED / SEPARATE

5. ADA-ACCESS-RUNTIME-COMPOSITION
   PLANNED / SEPARATE
```

El orden posterior al primer punto puede refinarse cuando cada frontera se verifique; no
mezclar esos frentes en el mismo incremento.

## Frentes separados que permanecen abiertos

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT

exact navigation fallback for authenticated non-promoted identities
PLANNED / SEPARATE

concrete Entra/Graph UsersDirectoryReader provider
UNVERIFIED

WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN

PYTHON-METADATA-ALIGNMENT
PLANNED / OPEN

CI remote
UNVERIFIED

full Ruff workspace
UNVERIFIED
```
