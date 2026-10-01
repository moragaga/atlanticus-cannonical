# ADA Command Center — Open Items

Estado: **CURRENT — MANAGER ADMINISTRATION AND GENERIC APPLICATION CLOSED / DUAL-PRODUCT TOOLING-DISTRIBUTION NEXT / RESOURCE PREPARATION DEFERRED.**

## Authority checkpoint

```text
Implementation CURRENT
moragaga/atlanticus:main@736ae9820878a5d8ec7fa7f922ce483be3d3e6b3

Decisions
moragaga/atlanticus-decisions:main@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e

Canonical base before this replacement
moragaga/atlanticus-cannonical:main@226bcded9eb6970292c7722b23b595e93b4a9cff
```

## CLOSED in this sequence

```text
atlanticus-web-manager==0.3.19
→ CURRENT authority

Command Center Alarm Configuration Manager alignment
→ CLOSED / CURRENT

Command Center Tool Catalog Manager alignment
→ CLOSED / CURRENT

Command Center Configuration Manager 0.1.2 administration composition
→ CLOSED / CURRENT

Users Manager adoption in Command Center administration
→ CLOSED / CURRENT

Profiles Manager adoption in Command Center administration
→ CLOSED / CURRENT

Navigation Manager adoption in Command Center administration
→ CLOSED / CURRENT

Command Center Generic Application 0.1.0
→ CLOSED / CURRENT product composition root
```

Qualification evidence for the two latest increments:

```text
Configuration Manager 0.1.2
30 PASS
Ruff check PASS
Ruff format check PASS
git diff --check PASS

Generic Application 0.1.0
6 PASS
Ruff check PASS
Ruff format check PASS
git diff --check PASS
local smoke PASS reported by user
```

## NEXT único

```text
ADA-COMMAND-CENTER-DUAL-PRODUCT-TOOLING-DISTRIBUTION
PLANNED / NEXT
```

Reason:

A real Command Center product target now exists. The previous blocker for dual-product tooling/distribution is closed.

The next chat must begin with design/audit against existing tooling and answer only from actual code:

```text
what artifact/distribution tooling exists
which assumptions are hard-coded to ADA
how product target is selected
what each product distribution contains
entrypoints/scripts
package/path dependency closure
.env.detail inclusion/validation
artifact qualification
whether Command Center can be distributed without unintended ADA product coupling
```

Do not create a second tooling stack if the existing one can be safely generalized.

## OPEN after this close

| Element | Estado | Motivo |
|---|---|---|
| Dual-product tooling/distribution | PLANNED / NEXT | Generic product target now exists; tooling has not yet been audited in this hito. |
| Cross-product dependency audit (`scopes/ada` reused by Command Center) | OPEN / MUST OBSERVE DURING TOOLING | Existing implementation uses `AdaStorageNamespace` and `ada-web-tools`; legitimacy must be established from code before change. |
| Manager header/branding specific to Command Center | PLANNED / DEFERRED | Current generic Manager header accepted by user; not part of tooling. |
| Production identity / Entra binding | PLANNED / UNVERIFIED | Generic 0.1.0 launcher is local-only and fails fast in production. |
| Operational UsersRuntime binding | OPEN / NOT IMPLEMENTED | Users Manager exists, but Generic 0.1.0 does not wire shared operational UsersRuntime. |
| Durable Users topology | PLANNED / UNFROZEN | Local stores are in-process. |
| Durable Profiles topology | PLANNED / UNFROZEN | Local active projection is in-process. |
| Durable Navigation topology | PLANNED / UNFROZEN | Local active projection is in-process. |
| Resource Preparation + startup gate | PLANNED / DEFERRED | Important but not immediate next focus. |
| Tool Catalog fully local filesystem | OPEN | Local provider still requires Storage for confirmed catalog. |
| C3 real GREEN qualification | BLOCKED BY DESIGN | Definitive producer/verifiers not closed. |
| C5 technical evidence/env | PLANNED | Final owner/key/version still pending. |
| Docker/runtime final Command Center | UNVERIFIED | Local product host works; container/deployment artifact is not qualified. |
| Alarm Source/Projection physical E2E | UNVERIFIED | Contracts do not prove identical physical resource across all hosts. |
| Live Projection | PLANNED | Not implemented. |
| Management Capture/Projection | PLANNED / SEPARATE | Keep separate. |
| History/Analytics | PLANNED / SEPARATE | Do not infer from FACTS/CURRENT. |
| Operational alarm read model/Home | PLANNED / SEPARATE | Current Home is minimal and does not render engine state. |
| END_OF_SHIFT operational resolution | PLANNED / UNVERIFIED | Real source/consumer remains unresolved. |
| Existing Alarm authoring UX defects | OPEN / SEPARATE | Separate focus. |
| Python 3.14.7 / Trixie migration | BLOCKED / DEFERRED | Current packages still declare 3.14.2; migration not opened. |

## SUPERSEDED / refined sequencing

SUPERSEDED:

```text
Command Center Generic Application
→ NEXT
```

It is now CURRENT.

SUPERSEDED as sequencing guidance:

```text
Resource Preparation + startup gate
→ ÚNICO PRÓXIMO FOCO
```

Resource Preparation remains open but is no longer the next focus.

CURRENT sequence:

```text
Manager convergence                                  CLOSED
Command Center administration composition            CLOSED
Command Center Generic Application                   CLOSED / CURRENT
dual-product tooling/distribution                    PLANNED / NEXT
Resource Preparation / startup gate                  PLANNED / DEFERRED
```

The old external file:

```text
ada_command_center_foundation_increment.zip
```

remains:

```text
SUPERSEDED / DO NOT APPLY
```

## Do not open during the next focus

```text
Live
Analytics
History
Management Capture
Alarm Engine redesign
Manager branding
production identity implementation
durable Users/Profiles/Navigation implementation
Python/Trixie migration
new compatibility aliases
new configuration contracts without observed need
```
