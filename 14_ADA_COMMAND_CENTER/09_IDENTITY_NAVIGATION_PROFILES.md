# ADA Command Center — Identity, Users, Profiles, Navigation and Manager

Estado: **CURRENT DIRECTION / MANAGER AUTHORIZATION CONVERGED / NAVIGATION COMPOSITION BLOCKED**

## Identity

Producción usa Microsoft Entra ID mediante la capability transversal Atlanticus.

No crear autenticación paralela.

La identidad autenticada, Users, Profiles, Navigation y Manager son contratos separados.

## Users

Users es generic Atlanticus.

Ownership:

```text
user → profile_key
```

No contiene ADA-specific access state.

`local` permanece un perfil/identidad especial de runtime local, no una asignación managed que deba sembrarse para poblar UI.

Users Manager composition:

```text
web/compositions/users-manager
CURRENT / VERIFIED
```

## Profiles

Profiles es generic Atlanticus.

Ownership:

```text
profile definitions
ProfileCatalog
Source
Projection
```

Profiles Manager composition:

```text
web/compositions/profiles-manager
CURRENT / VERIFIED
```

No agregar permisos de producto a Profiles.

## Navigation

Navigation es generic Atlanticus.

Durable:

```text
allowed_profiles = profile keys
```

Navigation no depende de Users ni de ADA Access.

La validación contra perfiles debe ocurrir mediante un contrato neutral provisto por la product composition:

```text
ProfileCatalog
    ↓ product composition
NavigationProfileOption
    ↓
Navigation Configuration
```

Por tanto queda congelado:

```text
Navigation Configuration package → Profiles package
FORBIDDEN

Product composition → Profiles + Navigation contracts
ALLOWED / CURRENT DIRECTION
```

No persistir copias de `ProfileDefinition` dentro de Navigation.

## Navigation Manager reusable — finding CURRENT

Existe:

```text
web/compositions/navigation-manager
```

pero ADA no lo consume hoy.

Estado:

```text
BLOCKED / CONVERGENCE REQUIRED
```

Findings VERIFIED:

- usa `ManagerAuthorizationPolicy.can_access(...)` aunque el contrato CURRENT expone `can_view(...)`;
- registra servicios sobre un `ServiceRegistry` recibido por la composition;
- ADA usa wiring/workflows propios;
- workflows generic y ADA difieren en validación/concurrencia/workspace;
- generic default `SourceKey` es `navigation-configuration`;
- ADA usa `navigation`;
- providers generic no exponen hoy toda esa variación de forma equivalente;
- labels runtime generic están fijados.

No migrar ADA ni Command Center hasta decidir el contrato único.

No introducir alias entre `navigation` y `navigation-configuration`.

## Manager authorization

Manager Core CURRENT:

```text
access_key=None                  → DENY
administrative_override=True     → ALLOW
matching granular access_key     → ALLOW
otherwise                        → DENY
```

`ManagerPrincipal.administrative_override` es Manager authority real.

La product composition decide cuándo emitir override.

No derivarlo desde ADA Access.

Command Center no debe incorporar ADA Access.

## Bindings legítimos vs adapters legacy

Permitido:

```text
ManagerPrincipal
    ↓ product composition
NavigationPrincipal
```

Permitido:

```text
ProfileCatalog
    ↓ product composition
NavigationProfileOption
```

Prohibido:

```text
old contract
    ↓ compatibility shim/alias
new contract
```

No mantener dos autoridades después de una convergencia.

## Próximo frente

```text
MANAGER-COMPOSITION-CONVERGENCE-AND-DUAL-PRODUCT-INTEGRATION
NEXT
```

Primero converger Navigation/Manager reusable; luego ADA Generic; luego Command Center; luego tooling/distribution de ambos.
