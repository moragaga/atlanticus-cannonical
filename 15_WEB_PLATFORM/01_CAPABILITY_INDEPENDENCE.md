# Web Platform — Capability Independence

Estado: **CURRENT / COMPOSITION BOUNDARIES RECONCILED**

## Regla

Separar:

```text
domain/core dependency
application-specific dependency
composition-only integration
```

No crear dependencias por simetría.

No usar adapters legacy para ocultar una frontera mal definida.

## Users

Users es generic Atlanticus.

Ownership:

```text
user → profile_key
```

Users puede validar managed profile keys mediante contratos de Profiles cuando esa responsabilidad pertenezca al servicio de administración.

Users no posee:

```text
ADA Access
Navigation
Tool configuration
KPI
Manager authorization
```

Manager integration:

```text
web/compositions/users-manager
→ ManagerEntry
```

Estado:

```text
CURRENT / VERIFIED / consumed by ADA
```

## Profiles

Profiles es generic Atlanticus.

Ownership:

```text
ProfileDefinition
ProfileCatalog
Source
Projection
```

Manager integration:

```text
web/compositions/profiles-manager
→ ManagerModule
```

Estado:

```text
CURRENT / VERIFIED / consumed by ADA
```

Profiles no contiene permisos ADA ni Manager.

## ADA Access

ADA Access es application-specific ADA.

Ownership:

```text
profile_key → ADA operational access_keys
```

No posee `user → profile_key`.

No alimenta `ManagerPrincipal.access_keys`.

Command Center no depende de ADA Access.

## Navigation

Navigation es generic Atlanticus.

Ownership:

```text
route structure
allowed_profiles
NavigationPrincipal
operational route authorization
```

Prohibido:

```text
Navigation → Users
Navigation → ADA Access
Navigation Configuration package → Profiles package
```

La integración de perfiles ocurre en product composition mediante un contrato neutral:

```text
ProfileCatalog
    ↓ composition
NavigationProfileOption
```

Esto es `composition-only integration`, no dependencia de Navigation sobre Profiles.

## Manager

Manager es generic Atlanticus.

Ownership:

```text
administrative shell
registry
ManagerModule / ManagerEntry
administrative authorization
common workflow
```

Manager authorization no se deriva de ADA Access ni Navigation.

Package/version authority CURRENT:

```text
web/capabilities/manager
atlanticus-web-manager==0.3.18
```

## Manager compositions auditadas

```text
users-manager
CURRENT / VERIFIED

profiles-manager
CURRENT / VERIFIED

navigation-manager
BLOCKED / incomplete convergence
```

Navigation Manager reusable no es consumida por ADA actualmente.

No puede tratarse como autoridad final hasta resolver:

```text
can_access vs can_view
ServiceRegistry lifecycle
workflow behavior
source_key
provider/runtime labels
```

## Product composition

Una product composition puede conocer varias capabilities y conectar sus contratos explícitos.

Ejemplos válidos:

```text
Profiles → neutral options → Navigation
ManagerPrincipal → binding → NavigationPrincipal
```

Eso no autoriza imports inversos dentro de las capabilities.

## No legacy

Después de una convergencia:

```text
one owner
one contract
one version authority
```

No mantener:

```text
old implementation + adapter + new implementation
duplicate access semantics
aliases for renamed Source keys
parallel Manager versions
```

## Próximo foco

```text
MANAGER-COMPOSITION-CONVERGENCE-AND-DUAL-PRODUCT-INTEGRATION
PLANNED / NEXT
```

El mismo frente termina en ADA Generic + Command Center + tooling/distribution de ambos.
