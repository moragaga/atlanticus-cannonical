# Web Platform — Open Items

Estado: **PLANNED OPEN ITEMS**

Los items cerrados no deben reabrirse para restaurar simetría o legacy.

## Closed baselines relevantes

```text
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

## ADA Access Projection persistence — NEXT

```text
ADA-ACCESS-PROJECTION-PERSISTENCE
PLANNED / NEXT / DESIGN FIRST
```

Antes de implementar:

```text
inspect ProjectionStore / ProjectionRecord / ProjectionTarget
inspect Profiles projection-local / projection-cosmos
inspect Profiles durable serializer
inspect ADA Access source_projection CURRENT
```

No copiar el serializer de Profiles sin verificar `dependencies`.

No asumir package names, Cosmos topology o durable schema antes del diseño.

## navigation-manager authorization consumer

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

No crear compatibility alias.

## Users Administration

```text
USERS-ADMINISTRATION-SURFACE-CUTOVER
PLANNED / SEPARATE
```

Usar `UserRecord.profile_key`, `UsersAdministrationService` y `ProfileCatalog`.

## ADA Access Configuration UI

```text
PLANNED / SEPARATE
```

Usar `AdaAccessConfiguration` CURRENT.

No reintroducir `UserProfileAssignment`.

## Manager final administrative composition

```text
MANAGER-FINAL-ADMIN-COMPOSITION
PLANNED / SEPARATE
```

Profiles Manager composition reusable ya existe.

## Navigation runtime fallback

```text
PLANNED / SEPARATE
```

No crear fictitious UserRecord ni Navigation -> Users/ADA Access dependency.

## ADA Access runtime

```text
PLANNED / SEPARATE
```

## Entra / Directory

```text
concrete provider
UNVERIFIED
```

No inventar Graph settings/scopes/endpoints.

## Test contract cleanup

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN
```

## Python baseline

```text
PYTHON-METADATA-ALIGNMENT
PLANNED / OPEN
```

## Qualification transversal

```text
CI remote
UNVERIFIED

full Ruff workspace
UNVERIFIED
```
