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

PROFILES-ADA-CONFIGURATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

USERS-PROFILES-CONTRACT-REALIGNMENT
CLOSED / VERIFIED / CURRENT

USERS-ADMINISTRATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROFILE-OWNERSHIP-REALIGNMENT
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROJECTION-CONTRACT
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROJECTION-PERSISTENCE
CLOSED / VERIFIED / CURRENT
```

## ADA Access Configuration Manager integration — NEXT

```text
ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
PLANNED / NEXT / DESIGN FIRST
```

Antes de implementar:

```text
inspect ADA Access core models CURRENT
inspect AdaAccessConfiguration CURRENT
inspect AdaAccessSourceService CURRENT
inspect AdaAccessProjectionBuilder CURRENT
inspect projection-local / projection-cosmos CURRENT
inspect ManagerModule / ManagerEntry CURRENT
inspect ADA Configuration Manager composition CURRENT
inspect real access-key consumers before defining identifier semantics
```

No existe Web surface Access CURRENT.

No copiar una UI de otro módulo por simetría.

No asumir todavía si la superficie final se materializa exclusivamente como `ManagerModule`
o requiere contracts adicionales; Access sí tiene Source/Projection reales, pero el nuevo
requisito de definición de access permissions debe resolverse primero contra el dominio
CURRENT.

### Requerimiento de producto a diseñar

El flujo esperado es manual/controlado:

```text
crear/definir un acceso
→ obtener un identificador estable
→ asignarlo a uno o más Profiles
→ la runtime projection entrega profile -> access identifiers
→ el desarrollador usa manualmente el identificador para proteger/habilitar funcionalidades
```

CURRENT ya resuelve:

```text
profile_key -> access_keys
```

CURRENT no resuelve explícitamente:

```text
catálogo/definición de access permissions
creación de access permissions
metadata de access permissions
identidad durable de una definición distinta de la string access_key
```

La forma exacta permanece OPEN.

No implementar autodescubrimiento ni modificación automática del código Web.

## Manager final administrative composition

```text
MANAGER-FINAL-ADMIN-COMPOSITION
PLANNED / AFTER ADA ACCESS
```

Users y Profiles ya están integrados.

## navigation-manager authorization consumer

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

No crear compatibility alias.

## ADA Access runtime

```text
PLANNED / SEPARATE
```

No mezclar con el editor/Manager.

## Navigation runtime fallback

```text
PLANNED / SEPARATE
```

No crear fictitious UserRecord ni Navigation -> Users/ADA Access dependency.

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
