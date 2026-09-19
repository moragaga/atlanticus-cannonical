# Web Platform — Capability Independence

Estado: **CURRENT / REFINED AFTER USERS-PROFILES REALIGNMENT**

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

## Profiles

Profiles es first-class generic Atlanticus capability.

CURRENT:

```text
profiles/core
profiles/configuration
profiles/projection-local
profiles/projection-cosmos
```

y existe:

```text
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

Ownership:

```text
profile_key -> access_keys
```

No posee user-to-profile assignment.

No convertir ADA Access en dependency de Navigation.

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

Manager registra módulos disponibles en composition.

Users no es `ManagerModule` Source/Projection.

Profiles sí dispone de composition Manager porque tiene Configuration Source/Projection
reales.

La aplicación final administrativa no debe considerarse completa sólo por existir esa
composition reusable.

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
moragaga/atlanticus@a31fce11d26a7c0a554d82de1813a4311522919b
```

CLOSED / VERIFIED / CURRENT:

```text
PROFILES-MANAGER-COMPOSITION
USERS-PROFILES-CONTRACT-REALIGNMENT
ADA-ACCESS-PROFILE-OWNERSHIP-REALIGNMENT
ADA-ACCESS-PROJECTION-CONTRACT
```

Siguiente gap recomendado:

```text
ADA-ACCESS-PROJECTION-PERSISTENCE
PLANNED / NEXT / DESIGN FIRST
```
