# Web Platform — Users / Profiles / Navigation / Manager Capability Boundary

Estado: **CURRENT / MANAGER AUTHORIZATION CLOSED / NAVIGATION MANAGER CONVERGENCE BLOCKED**

Implementation:

```text
moragaga/atlanticus@a75465745e188da4765e803595b17acaa55d9306
```

## Ownership

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

## Navigation Manager composition

Estado auditado:

```text
web/compositions/navigation-manager
BLOCKED
```

No es CURRENT authority para consumidores nuevos.

Findings VERIFIED:

1. ADA no la consume.
2. Llama `ManagerAuthorizationPolicy.can_access(...)`; CURRENT expone `can_view(...)`.
3. Registra Source/Projection/Validation services directamente sobre un `ServiceRegistry` externo.
4. Profiles/Alarm compositions registran esos services mediante `WebModule.register_services`.
5. Workflow generic añade validadores y checks que no son idénticos al workflow ADA.
6. Default source key generic: `navigation-configuration`.
7. ADA source key: `navigation`.
8. No introducir alias para esconder esta diferencia.
9. Runtime source/projection labels generic no están alineados con la variación que otros compositions ya exponen.

Antes de modificar, decidir explícitamente:

```text
source_key authority
workflow validation/concurrency authority
workspace semantics
authorization call
service lifecycle
runtime labels/provider API
```

Luego reemplazar limpiamente el wiring anterior.

## Authority/version

Manager package owner:

```text
atlanticus-web-manager==0.3.18
web/capabilities/manager
```

El próximo frente debe terminar con una única versión/authority consumida por ADA, Command Center y tooling/distribution.

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

```text
MANAGER-COMPOSITION-CONVERGENCE-AND-DUAL-PRODUCT-INTEGRATION
```

Same chat:

```text
generic convergence
→ ADA Generic
→ Command Center
→ tooling/distribution both
→ qualification
```
