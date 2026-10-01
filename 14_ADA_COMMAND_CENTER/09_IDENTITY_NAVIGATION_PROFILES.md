# ADA Command Center — Identity, Users, Profiles, Navigation and Manager

Estado: **CURRENT DIRECTION / MANAGER AUTHORITY CONVERGED / USERS-PROFILES-NAVIGATION NOT YET REQUIRED BY CURRENT HOST**

## Authority of this close

```text
Last confirmed Atlanticus HEAD:
moragaga/atlanticus@36361dd570f86e8350ea4a6ee0e09bab351ba171

Command Center Manager 0.3.19 delta:
VERIFIED LOCAL / PENDING FINAL GIT HEAD
```

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

Command Center CURRENT no la consume todavía.

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

Command Center CURRENT no la consume todavía.

## Navigation

Navigation es generic Atlanticus.

Durable:

```text
allowed_profiles = profile keys
```

Navigation no depende de Users ni de ADA Access.

Cuando un producto necesite validar perfiles:

```text
ProfileCatalog
    ↓ product composition
NavigationProfileOption
    ↓
Navigation Configuration
```

Congelado:

```text
Navigation Configuration package → Profiles package
FORBIDDEN

Product composition → Profiles + Navigation contracts
ALLOWED
```

No persistir copias de `ProfileDefinition` dentro de Navigation.

## Navigation Manager reusable — CURRENT

Authority:

```text
web/compositions/navigation-manager
atlanticus-web-composition-navigation-manager==0.3.0
CURRENT / CONVERGED
```

Las divergencias históricas quedaron resueltas:

```text
authorization       → can_view(...)
service lifecycle   → WebModule.register_services
source_key          → configurable
runtime labels      → configurable
workspace           → ManagerWorkspaceBinding
profile validation  → neutral NavigationProfileOption provider
```

ADA ya consume esta composition.

Command Center no debe adoptarla sólo para igualar a ADA. El host actual no tiene shell operacional/navigation que la requiera. Su necesidad se decide dentro del futuro `ada-command-center-generic-application`.

## Manager authorization

Manager Core CURRENT:

```text
atlanticus-web-manager==0.3.19
```

Contrato:

```text
access_key=None                  → DENY
administrative_override=True     → ALLOW
matching granular access_key     → ALLOW
otherwise                        → DENY
```

`ManagerPrincipal.administrative_override` es Manager authority real.

La product composition decide cuándo emitir override.

No derivarlo desde ADA Access.

Command Center no incorpora ADA Access.

## Command Center Manager CURRENT

Capabilities administrativas existentes:

```text
Alarm Configuration Manager
Tool Catalog Manager
temporary Configuration Manager host
```

La convergencia local cerró:

```text
ada-command-center-web-alarm-configuration 0.1.1
ada-command-center-web-tool-catalog-manager 0.1.1
ada-command-center-configuration-manager 0.1.1
atlanticus-web-manager 0.3.19
```

`AlarmConfigurationManagerWorkspaceBinding` conserva únicamente especialización de dominio:

```text
Save Draft
→ require Confirmed Tool Catalog
→ pin current catalog revision
→ delegate workspace mechanics to ManagerWorkspaceBinding
```

Ownership, SourceKey, snapshot y serialización de workspace pertenecen al Manager core.

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

## NEXT

No agregar Users/Profiles/Navigation en aislamiento.

El próximo foco único es:

```text
ADA-COMMAND-CENTER-GENERIC-APPLICATION-COMPOSITION
PLANNED / NEXT
```

Ese diseño debe determinar qué capabilities necesita realmente el composition root final de Command Center.
