# Web Platform — Users / Profiles / Navigation / Manager Capability Boundary

Estado: **CURRENT / MANAGER AUTHORIZATION CLOSED / NAVIGATION MANAGER CONVERGENCE CLOSED**

## Autoridad de este cierre

```text
Último HEAD Atlanticus confirmado en este chat:
moragaga/atlanticus@36361dd570f86e8350ea4a6ee0e09bab351ba171

Delta posterior Command Center:
VERIFIED LOCAL / PENDING FINAL GIT HEAD
```

La delta local posterior al HEAD confirmado no cambia el contrato transversal de esta página; alinea Command Center con la misma autoridad `atlanticus-web-manager==0.3.19`.

## Ownership CURRENT

```text
Atlanticus Users
identity/lifecycle + user → profile_key

Atlanticus Profiles
profile definitions + catalog + Source + Projection

Atlanticus Navigation
route structure + allowed_profiles + operational authorization

Atlanticus Manager
administrative shell + administrative authorization

ADA Access
ADA-only profile_key → operational access_keys

ADA Generic / Command Center
product composition
```

## System profiles

```text
basic
root
guest
local
```

`local` es runtime local especial.

No sembrar Users managed sólo para representar identidades locales.

## Manager authorization

Contrato CURRENT:

```text
access_key is None                    → DENY
administrative_override=True          → ALLOW
access_key in principal.access_keys   → ALLOW
otherwise                             → DENY
```

Product composition:

```text
managed root
→ override

trusted local + local environment
→ override

basic / guest / custom
→ no Manager administration

bootstrap root
→ no implicit Manager administration
```

Manager Core no infiere override desde profile/is_local.

## ADA Access

ADA Access es exclusivamente operacional ADA.

```text
profile_key → access_keys
```

No se proyecta a Manager.

No existe en Command Center.

## Navigation + Profiles

Navigation no importa Profiles.

La product composition adapta únicamente los datos que Navigation necesita:

```text
ProfileCatalog
    ↓
NavigationProfileOption(key, label)
```

Congelado:

```text
Navigation Configuration → Profiles package
FORBIDDEN

Product composition → Profiles contract
ALLOWED

Product composition → Navigation neutral option contract
ALLOWED
```

## Navigation Manager composition — CLOSED / CURRENT

Autoridad reusable:

```text
web/compositions/navigation-manager
atlanticus-web-composition-navigation-manager==0.3.0
```

La convergencia resolvió las divergencias previamente auditadas:

```text
authorization call          → ManagerAuthorizationPolicy.can_view(...)
service lifecycle           → WebModule.register_services
source_key                  → configurable; ADA injecta SourceKey('navigation')
runtime labels              → source_name / projection_name configurables
workspace mechanics         → ManagerWorkspaceBinding
profile validation          → NavigationProfileOption provider neutral
```

ADA consume la composition reusable. El wiring/workflows bespoke de Navigation que vivía en ADA Configuration Manager quedó reemplazado limpiamente; no se conservaron aliases ni service IDs ADA antiguos.

La composición reusable conserva su default genérico `navigation-configuration`; ADA inyecta explícitamente `navigation`. No existe alias entre ambas identidades.

## Manager authority/version

Package owner:

```text
web/capabilities/manager
atlanticus-web-manager==0.3.19
```

Estado:

```text
Manager 0.3.18                         SUPERSEDED
Manager 0.3.19                         CURRENT

Navigation Manager composition 0.2.0   SUPERSEDED
Navigation Manager composition 0.3.0   CURRENT / CLOSED
```

ADA quedó integrado con esta autoridad. Command Center quedó localmente calificado con la misma versión; falta únicamente registrar el HEAD Git final de esa delta si todavía no fue integrado.

## Product-specific state

### ADA

```text
Users Manager       CURRENT / consumed
Profiles Manager    CURRENT / consumed
Navigation Manager  CURRENT / consumed
Manager Core 0.3.19 CURRENT
```

ADA conserva separadas:

```text
administrative Navigation composition
operational Navigation projection consumption
```

`ConfigurationManagerDependencies.navigation_projection_store` es dependencia operacional real de ADA Generic; no es compatibilidad legacy.

### ADA Command Center

Command Center actualmente sólo compone las capabilities administrativas que existen:

```text
Alarm Configuration Manager
Tool Catalog Manager
temporary Configuration Manager host
```

Users, Profiles y Navigation no se agregan por simetría con ADA. Su integración queda sujeta a una necesidad real del futuro composition root de Command Center.

## Reglas congeladas

```text
Atlanticus generic core → ADA-specific dependency     FORBIDDEN
Users → profile_key                                  CURRENT
ADA Access → Manager permissions                     FORBIDDEN
Navigation → Users                                   FORBIDDEN
Navigation → ADA Access                              FORBIDDEN
Navigation Configuration → Profiles package          FORBIDDEN
composition neutral profile binding                  CURRENT
is_local alone → Manager administration              FORBIDDEN
root profile alone inside Manager Core               FORBIDDEN
composition-owned administrative_override            CURRENT
granular Manager access_keys                         CURRENT
access_key=None                                      DENY
legacy shims / aliases / duplicate contracts         FORBIDDEN
parallel Manager versions                            FORBIDDEN
```

## NEXT

La convergencia de Manager ya no es el siguiente trabajo.

El siguiente foco de producto es:

```text
ADA-COMMAND-CENTER-GENERIC-APPLICATION-COMPOSITION
PLANNED / NEXT
```

Primero definir el composition root real de Command Center. Sólo después de existir y quedar calificado un producto Command Center integrable corresponde retomar alineación dual de tooling/distribution.
