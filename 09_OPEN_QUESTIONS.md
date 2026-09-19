# Atlanticus — Open Questions

Estado: **CANONICAL OPEN ITEMS**

Los puntos aquí no reabren contracts CLOSED.

## CLOSED — Profiles editor / Web surface / Projection / Manager composition

```text
PROFILES-CONFIGURATION-EDITOR-CONTRACT
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-WEB-SURFACE
CLOSED / VERIFIED / CURRENT

PROFILES-PROJECTION-CONTRACT
CLOSED / VERIFIED / CURRENT

PROFILES-MANAGER-COMPOSITION
CLOSED / VERIFIED / CURRENT
```

No reabrirlos para rediseñar Profiles mientras se implementan consumers.

## CLOSED — Users / Profiles contract realignment

```text
USERS-PROFILES-CONTRACT-REALIGNMENT
CLOSED / VERIFIED / CURRENT
```

CURRENT:

```text
UserRecord.profile_key
EffectiveUser.profile_key
Users -> ProfileCatalog
```

REMOVED:

```text
authority_key
authority.py
```

## CLOSED — ADA Access ownership

```text
ADA-ACCESS-PROFILE-OWNERSHIP-REALIGNMENT
CLOSED / VERIFIED / CURRENT
```

CURRENT:

```text
profile_key -> access_keys
```

REMOVED:

```text
UserProfileAssignment
user_id -> profile_keys
```

## CLOSED — ADA Access Projection contract

```text
ADA-ACCESS-PROJECTION-CONTRACT
CLOSED / VERIFIED / CURRENT
```

CURRENT:

```text
AdaAccessProjectionBuilder
create_ada_access_projection_service
ProjectionRecord[AdaAccessConfiguration]
exact dependency -> Profiles ProjectionTarget
```

## OPEN — ADA Access Projection persistence

```text
ADA-ACCESS-PROJECTION-PERSISTENCE
PLANNED / NEXT / DESIGN FIRST
```

Debe verificarse primero el contract genérico de `ProjectionStore`/`ProjectionRecord`, los
providers de Profiles y la serialización existente.

Punto crítico a resolver antes de implementar:

```text
ProjectionRecord.dependencies
```

ADA Access sí tiene una dependencia exacta de Profiles; cualquier persistencia durable debe
preservar la provenance necesaria para reconstruir el mismo record/target.

No asumir package names, topology Cosmos ni schema durable sin verificar.

## OPEN — navigation-manager authorization consumer mismatch

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

No introducir alias de compatibilidad.

## OPEN — Navigation fallback para identidad no promovida

La autenticación base no requiere promoted `UserRecord`.

Sigue OPEN la composición exacta de Navigation para esa identidad.

No crear Users record ficticio ni dependencias Navigation -> Users/ADA Access.

## OPEN — Users Administration surface

```text
USERS-ADMINISTRATION-SURFACE-CUTOVER
PLANNED / SEPARATE
```

Consumir `UsersAdministrationService` y el current `profile_key` contract.

## OPEN — ADA Access Configuration UI

```text
PLANNED / SEPARATE
```

Consumir `AdaAccessConfiguration` CURRENT; no reintroducir user-to-profile ownership.

## OPEN — final Manager administrative composition

```text
MANAGER-FINAL-ADMIN-COMPOSITION
PLANNED / SEPARATE
```

Profiles Manager composition reusable existe; la aplicación final todavía debe integrar las
superficies que correspondan cuando sus fronteras estén cerradas.

## OPEN — ADA Access runtime composition

```text
PLANNED / SEPARATE
```

No convertir ADA Access en dependency de Navigation.

## OPEN — concrete Entra directory discovery

```text
UsersDirectoryReader
CURRENT CONTRACT

Graph/Entra provider
UNVERIFIED
```

No inventar settings/scopes/endpoints.

## OPEN — Python metadata alignment

```text
PYTHON-METADATA-ALIGNMENT
PLANNED / OPEN
```

## PLANNED — Web test contract cleanup

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN
```

## UNVERIFIED

```text
CI remote
full Ruff workspace
Python/Trixie global qualification
concrete Entra/Graph provider
```
