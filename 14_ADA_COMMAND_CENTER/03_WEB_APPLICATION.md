# ADA Command Center — Web Application

Estado: **CURRENT — GENERIC PRODUCT COMPOSITION ROOT IMPLEMENTED / VERIFIED LOCAL; CONFIGURATION MANAGER REMAINS SEPARATE QUALIFICATION APP; TOOLING/DISTRIBUTION NEXT.**

## Authority of this close

```text
Implementation CURRENT
moragaga/atlanticus:main@736ae9820878a5d8ec7fa7f922ce483be3d3e6b3

Administration composition
moragaga/atlanticus@2ccc2dffd792d55ae68aee3d64ed73eef408bbf8

Generic Application
moragaga/atlanticus@736ae9820878a5d8ec7fa7f922ce483be3d3e6b3
```

## Capabilities Web CURRENT

```text
scopes/ada-command-center/web/
  alarms/configuration
  alarms/persistence
  alarms/projection-local
  alarms/projection-cosmos
  tools/catalog
  tools/discovery-cosmos
  tools/catalog-manager
  application/ada-command-center-configuration-manager
  application/ada-command-center-generic-application
```

## Application roles — FROZEN

```text
ada-command-center-generic-application
→ real product composition root

ada-command-center-configuration-manager
→ separate development/testing/qualification application
```

The configuration-manager application is not to be absorbed, executed or treated as a remote service by Generic.

The Generic package may depend on and reuse composition/contracts exposed by the configuration-manager package. This is the same application-role pattern already established in ADA; it does not imply identical product internals.

## Administrative composition — CURRENT

`ada-command-center-configuration-manager==0.1.2` exposes the composition used by both its standalone qualification host and Generic:

```text
Manager
├── Users
├── Profiles
├── Navigation
├── Tool Catalog
└── Alarm Configuration
```

The administration composition uses Atlanticus generic compositions for Users, Profiles and Navigation.

Tool Catalog and Alarm Configuration retain Command Center-specific behavior.

Alarm Configuration preserves its domain specialization:

```text
Save Draft
→ require Confirmed Tool Catalog
→ pin current catalog revision
→ delegate generic workspace mechanics to ManagerWorkspaceBinding
```

## Generic product host — CURRENT

`ada-command-center-generic-application==0.1.0` builds one `WebApplicationDefinition` with:

```text
Home /
Identity module
Projected Navigation module
Navigation authorization
Manager web modules
Surface router
Command Center pages
Manager pages
```

The layout exposes:

```text
operational surface
  → minimal Command Center header/navigation
  → page_container

manager surface
  → ManagerSurface layout
```

The surface router switches between `/manager...` and the operational surface.

The operational Navigation consumes `dependencies.administration.navigation_projection_store` using:

```text
NAVIGATION_SOURCE_KEY = SourceKey('navigation')
```

The same Manager principal is adapted to a neutral `NavigationPrincipal` through product composition. No ADA Access dependency is used.

## Identity — CURRENT LOCAL / PRODUCTION UNVERIFIED

Launcher 0.1.0 supports only the local Manager provider.

Local startup composes:

```text
LocalIdentityProvider
ManagerPrincipal(
  profile_keys=('local',),
  administrative_override=True,
  is_local=True
)
```

Production startup intentionally fails fast with:

```text
Production Command Center requires an injected production identity provider
```

Microsoft Entra ID remains the target production identity contract from the project baseline, but it is not wired into Generic 0.1.0.

Do not create a second identity system.

## Users / Profiles / Navigation — CURRENT boundary

Users, Profiles and Navigation are now required by explicit product composition and therefore the older need-driven non-adoption guidance is SUPERSEDED.

Current concrete behavior:

```text
Users
  → administrative Manager entry
  → local registry/promoted stores are in-process

Profiles
  → Manager module
  → Source persisted by LocalSourceStore
  → local active projection is in-process

Navigation
  → Manager module
  → Source persisted by LocalSourceStore
  → local active projection is in-process
  → operational surface consumes this projection
```

Do not claim durable Users/Profiles/Navigation or restart persistence of their in-process projection/admin state.

Generic 0.1.0 does not register a shared operational `UsersRuntime`; Users Manager being present does not prove that separate runtime binding.

## Explicitly excluded ADA capabilities

The current product decision excludes:

```text
ADA Access
ADA KPI Registry / Definition
ADA Collector
ADA Tool operational projection
ADA-specific shell
```

Do not import them into Command Center simply to match ADA Generic.

## Manager header presentation

Current Command Center composition does not set product-specific:

```text
header_brand_marks
header_title
header_subtitle
```

Therefore it uses the generic Manager header contract.

This visual difference is known and accepted for this close.

Product-specific Manager header/branding is:

```text
PLANNED / DEFERRED
```

Do not mix it into tooling/distribution.

## Qualification

Configuration Manager 0.1.2:

```text
30 PASS
Ruff check PASS
Ruff format check PASS
git diff --check PASS
```

Generic Application 0.1.0:

```text
6 PASS
Ruff check PASS
Ruff format check PASS
git diff --check PASS
local smoke PASS reported by user
```

No CI/Docker/Azure or durable production qualification is implied.

## Sequence — CURRENT

```text
Manager convergence reusable                           CLOSED
ADA Generic integration                                CLOSED
Command Center Manager administration composition      CLOSED
Command Center Generic Application                     CLOSED / CURRENT
dual-product tooling/distribution                      PLANNED / NEXT
Resource Preparation / startup gate                    PLANNED / DEFERRED
```

## Invariants

- Command Center Web is its own product; it is not a page of ADA Generic.
- `ada-command-center-generic-application` is the product composition root.
- `ada-command-center-configuration-manager` remains a separate qualification application.
- Reuse does not turn a logical boundary into a remote service.
- ADA Access does not belong to Command Center.
- Navigation does not depend directly on Users or ADA Access.
- Profiles and Navigation connect through neutral composition contracts.
- No compatibility shim/alias is introduced for replaced contracts.
- No Live/Analytics contract is inferred from the existence of the product host.
- No demo fixture becomes product state.
- Production identity and durable state remain explicit unresolved contracts.

## Python

Relevant current packages still declare:

```text
requires-python = "==3.14.2"
```

Project target baseline is 3.14.7/Trixie, but migration remains:

```text
BLOCKED / DEFERRED
```

until opened explicitly as its own focus.
