# ADA Command Center — Identity, Users, Profiles, Navigation and Manager

Estado: **CURRENT — GENERIC APPLICATION ADOPTS IDENTITY + USERS/PROFILES/NAVIGATION ADMINISTRATION + PROJECTED OPERATIONAL NAVIGATION; PRODUCTION IDENTITY AND DURABLE ADMINISTRATION TOPOLOGY REMAIN OPEN.**

## Authority of this close

```text
Implementation CURRENT
moragaga/atlanticus:main@736ae9820878a5d8ec7fa7f922ce483be3d3e6b3

Administration integration
moragaga/atlanticus@2ccc2dffd792d55ae68aee3d64ed73eef408bbf8

Generic Application
moragaga/atlanticus@736ae9820878a5d8ec7fa7f922ce483be3d3e6b3
```

## Identity

Identity remains a transversal Atlanticus capability.

Do not create authentication parallel to Atlanticus.

Project target for production remains Microsoft Entra ID, but Generic 0.1.0 currently wires only `LocalIdentityProvider`. Production startup fails fast until a production identity provider and durable runtime composition are explicitly injected.

Identity, Users, Profiles, Navigation and Manager remain separate contracts.

## Users — CURRENT administrative integration

Users is generic Atlanticus.

Ownership remains:

```text
user → profile_key
```

Users does not contain ADA-specific access state.

Current Command Center integration:

```text
web/compositions/users-manager
→ Users Manager entry
→ composed by ada-command-center-configuration-manager==0.1.2
→ surfaced inside Command Center Generic Manager
```

Current local Users stores are in-process.

Generic 0.1.0 does not register a shared operational `UsersRuntime`; do not equate Users Manager with an operational user runtime contract.

## Profiles — CURRENT administrative integration

Profiles remains generic Atlanticus.

Ownership:

```text
profile definitions
ProfileCatalog
Source
Projection
```

Current Command Center integration:

```text
web/compositions/profiles-manager
→ Profiles Manager module
→ Source LocalSourceStore in local runtime
→ active Projection in-process in local runtime
```

Do not add product permissions to Profiles.

Durable Profiles topology is not frozen.

## Navigation — CURRENT administrative + operational integration

Navigation remains generic Atlanticus.

Durable contract remains:

```text
allowed_profiles = profile keys
```

Navigation does not depend on Users or ADA Access.

Profile options remain connected only through composition:

```text
ProfileCatalog
    ↓ product composition
NavigationProfileOption
    ↓
Navigation Configuration
```

Frozen:

```text
Navigation Configuration package → Profiles package
FORBIDDEN

Product composition → Profiles + Navigation contracts
ALLOWED
```

No `ProfileDefinition` copies are persisted inside Navigation.

### Navigation Manager reusable

Authority:

```text
atlanticus-web-composition-navigation-manager==0.3.0
CURRENT
```

Current Command Center composition configures:

```text
source_key = 'navigation'
access_key = 'navigation.manage'
```

The Generic product application consumes the same administration navigation projection store through `create_projected_navigation_module(...)`.

A product binding converts `ManagerPrincipal` to neutral `NavigationPrincipal`.

No ADA Access dependency is introduced.

## Manager authorization — FROZEN

Manager Core:

```text
atlanticus-web-manager==0.3.19
```

Authorization contract:

```text
access_key=None                  → DENY
administrative_override=True     → ALLOW
matching granular access_key     → ALLOW
otherwise                        → DENY
```

`ManagerPrincipal.administrative_override` remains Manager authority.

Do not derive it from ADA Access.

## Command Center local principal — CURRENT

Generic 0.1.0 local runtime creates one principal aligned with local identity:

```text
subject_id=<same local identity subject>
display_name='Administrador local'
profile_keys=('local',)
administrative_override=True
is_local=True
```

This is local-runtime behavior only.

Do not infer production roles or permissions from it.

## Command Center Manager — CURRENT

Current administrative surface:

```text
Administration
├── Users
├── Profiles
└── Navigation

Configurations
├── Tool Catalog
└── Alarm Configuration
```

Relevant packages:

```text
ada-command-center-configuration-manager==0.1.2
atlanticus-web-manager==0.3.19
atlanticus-web-composition-users-manager==0.1.1
atlanticus-web-composition-profiles-manager==0.2.0
atlanticus-web-composition-navigation-manager==0.3.0
```

## Bindings legitimate vs legacy adapters — FROZEN

Allowed:

```text
ManagerPrincipal
    ↓ product composition
NavigationPrincipal
```

Allowed:

```text
ProfileCatalog
    ↓ product composition
NavigationProfileOption
```

Forbidden:

```text
old contract
    ↓ compatibility shim/alias
new contract
```

No parallel authority after a convergence.

## Decision refined in this close

Earlier guidance said Command Center should not adopt Users/Profiles/Navigation merely because reusable compositions existed.

That guidance was need-driven, not a prohibition.

It is now SUPERSEDED by the explicit product decision and implementation that the real Command Center Generic composition includes these capabilities.

The frozen exclusion remains:

```text
ADA Access
```

and no ADA-specific authorization state is introduced.

## OPEN

```text
Production Entra provider binding for Generic        PLANNED / UNVERIFIED
Durable Users topology                               PLANNED / UNFROZEN
Durable Profiles topology                            PLANNED / UNFROZEN
Durable Navigation topology                          PLANNED / UNFROZEN
Operational shared UsersRuntime                      NOT IMPLEMENTED
Manager product-specific header/branding             PLANNED / DEFERRED
```

Do not solve these inside dual-product tooling unless an existing artifact contract proves one is a hard blocker.
