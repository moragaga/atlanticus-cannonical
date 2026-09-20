# Web Platform — Capability Independence

Estado: **CURRENT / REFINED AFTER USERS MANAGER INTEGRATION**

## Regla

Independencia técnica no significa ausencia total de dependencias de dominio.

Separar:

```text
generic domain dependency
```

de:

```text
application-specific dependency
```

y de:

```text
composition-only integration
```

## Users

Users es generic Atlanticus.

CURRENT dependency:

```text
Users -> Profiles core
```

porque Users posee:

```text
user -> profile_key
```

y valida managed profile keys contra `ProfileCatalog`.

Users no depende de:

```text
ADA Access
Navigation
Tools
KPI
```

Global User no contiene ADA-specific access keys ni configuration de una aplicación.

Strong identity:

```text
issuer + subject_id
```

La Web surface administrativa Users permanece dentro de Users.

La integración al shell Manager ocurre mediante:

```text
web/compositions/users-manager
→ ManagerEntry
```

Esto es composition-only; no convierte Users en Source/Projection.

## Profiles

Profiles es first-class generic Atlanticus capability.

CURRENT:

```text
profiles/core
profiles/configuration
profiles/projection-local
profiles/projection-cosmos
web/compositions/profiles-manager
```

Profiles posee definiciones/catálogo; no posee ADA access permissions.

## ADA Access

ADA Access es application-specific.

CURRENT dependency:

```text
ADA Access -> Profiles core / Profiles Projection
```

para validar referencias exactas.

Ownership CURRENT:

```text
profile_key -> access_keys
```

No posee user-to-profile assignment.

No convertir ADA Access en dependency de Navigation.

CURRENT packages:

```text
scopes/ada/web/access/core
scopes/ada/web/access/configuration
scopes/ada/web/access/projection-local
scopes/ada/web/access/projection-cosmos
```

No existe Web surface ADA Access CURRENT.

CURRENT tampoco define un catálogo independiente de accesos: `access_keys` son strings
normalizadas almacenadas en grants por profile.

La necesidad de definir/crear accesos y producir un identificador estable consumible por
desarrolladores es una frontera de diseño futura, no un contract ya implementado.

## Navigation

Navigation continúa generic.

CURRENT:

```text
Navigation Configuration -> Profiles core
Navigation -> Users FORBIDDEN
Navigation -> ADA Access FORBIDDEN
Navigation Configuration -> Profiles Configuration FORBIDDEN
```

Durable:

```text
allowed_profiles = profile keys
```

## Runtime authorization composition

La composición exacta de Navigation para identidad autenticada no promovida sigue OPEN /
SEPARATE.

No resolver agregando dependencias directas Navigation -> Users/ADA Access.

## User Activity

User Activity conserva independencia funcional.

## Manager

Manager registra items administrativos disponibles en composition:

```text
ManagerModule
ManagerEntry
```

Users usa `ManagerEntry`.

Profiles usa `ManagerModule` porque posee Source/Projection reales.

La aplicación final administrativa no debe considerarse completa hasta integrar ADA Access
y cerrar explícitamente la composition final.

## Invariante estructural

Usar una dependencia core sólo cuando la responsabilidad real la requiere.

No crear fronteras por simetría.

CURRENT:

```text
Users -> Profiles core
JUSTIFIED BY user.profile_key

Navigation Configuration -> Profiles core
JUSTIFIED BY allowed_profiles

ADA Access -> Profiles
JUSTIFIED BY profile grants

Navigation -> Users/ADA Access
FORBIDDEN
```

## Estado de implementación

```text
moragaga/atlanticus@783d3578da52aeb5cf831999a7717dc8b79f2fb0
```

CLOSED / VERIFIED / CURRENT:

```text
PROFILES-MANAGER-COMPOSITION
USERS-PROFILES-CONTRACT-REALIGNMENT
USERS-ADMINISTRATION-MANAGER-INTEGRATION
ADA-ACCESS-PROFILE-OWNERSHIP-REALIGNMENT
ADA-ACCESS-PROJECTION-CONTRACT
ADA-ACCESS-PROJECTION-PERSISTENCE
```

Siguiente gap recomendado:

```text
ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
PLANNED / NEXT / DESIGN FIRST
```
