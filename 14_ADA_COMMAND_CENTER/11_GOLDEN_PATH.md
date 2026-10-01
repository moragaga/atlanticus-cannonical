# ADA Command Center — Golden Path

Estado: **PARTIALLY IMPLEMENTED — C1/C2/C4, authoring UX-01/UX-02, Alarm Projection naming and the Generic Web composition root are implemented under their local gates. Full resource preparation, durable production runtime, Live and operational alarm visualization remain NOT ACCREDITED.** Corte: 2026-10-01.

## 1. Authority and limits

```text
Implementation inspected
moragaga/atlanticus:main@736ae9820878a5d8ec7fa7f922ce483be3d3e6b3

Decisions inspected
moragaga/atlanticus-decisions:main@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e

Canonical base before replacement
moragaga/atlanticus-cannonical:main@226bcded9eb6970292c7722b23b595e93b4a9cff
```

Implementation is authority for existing code. User-reported local qualification is evidence, not CI/Docker/Azure qualification.

## 2. Current path, owners and gates

| Etapa | Owner actual | Estado demostrable |
|---|---|---|
| Tool Sources/Projections y named connections | Tool/Web | CURRENT; controlled local qualification historical, not Azure. |
| Confirmed Tool Catalog Cn | `web/tools/catalog` | CURRENT / C1 CLOSED in Blob. Local provider still uses Storage. |
| Discovery, inspect and human confirmation | `web/tools/discovery-cosmos` | CURRENT / C1 CLOSED. |
| Tool Catalog UI reusable | `web/tools/catalog-manager` | CURRENT / C1 CLOSED. |
| Alarm editor Rn/Cn, Save/Validate/Publish | Alarm Web/Domain | Source v3 CURRENT; UX-01/UX-02 closed under their gates. |
| Source Key `alarm-configuration` | Domain + Web/jobs | CURRENT / C2 CLOSED. |
| Alarm Projection physical name | Alarm Web/Projection | CURRENT / CLOSED: `alarm-configuration`; previous long name SUPERSEDED. |
| Alarm Projection local filesystem | Configuration Manager local | CURRENT for Alarm Projection. |
| Alarm Projection Cosmos | Web/Materialization | Contract CURRENT; physical multi-host equality UNVERIFIED. |
| `APPLICATION` common; independent leases and manual `VOLUMEN_PATH` | Alarm processes | CURRENT contractual; real shared mount UNVERIFIED. |
| Qualification Rn/Cn | Materialization | Current controlled/manual mechanism; C3 definitive producer not closed. |
| READY/BLOCKED exact pair | Materialization | CURRENT. |
| WAL adoption → EFFECTIVE | Persistence/Runtime | CURRENT; independent Docker qualification UNVERIFIED. |
| Engine CURRENT v1 + FACTS v2 | Runtime | CURRENT. |
| Delivery latest CURRENT with pin/READY/EFFECTIVE | `processes/alarms-delivery` | C4 CLOSED / CURRENT-only. |
| Command Center administration composition | Web Application | CURRENT: Users/Profiles/Navigation + Tool Catalog + Alarm Configuration. |
| Command Center Generic Application | Web Application | CURRENT `0.1.0`, locally qualified. |
| Generic Home `/` | Web Application | CURRENT minimal product home; it does not read Alarm Engine operational state. |
| Generic Manager `/manager` | Web Application | CURRENT locally. |
| Projected operational Navigation | Web Application | CURRENT locally from same administration projection store. |
| Dual-product tooling/distribution | Integration tooling | **PLANNED / NEXT**. |
| Resource Preparation + startup gate | Integration | PLANNED / DEFERRED. |
| Tool Catalog local filesystem | Web Tool Catalog | NOT IMPLEMENTED. |
| Production identity/durable administration | Web Application | PLANNED / UNVERIFIED. |
| Live materializer / `AlarmLiveProjection` | Backend Live | NOT IMPLEMENTED. |
| Management Capture/Projection and History/Analytics | Separate fronts | PLANNED. |
| Docker/Azure real | Integration | UNVERIFIED. |

## 3. Frozen chain contracts

```text
Tool Sources/Projections
  -> confirmed Tool Catalog Cn
  -> Alarm Configuration Source Rn + ToolDependencyManifest Cn (v3)
  -> Alarm Projection
       local:   conciencia_situacional/command-center/projections/alarm-configuration/...
       durable: Cosmos container alarm-configuration, PK /partition_key
  -> current qualification mechanism
  -> Materialization: BLOCKED or READY exact pair
  -> Engine WAL adoption / EFFECTIVE exact pin
  -> Engine CURRENT v1
  -> Engine FACTS v2 separate durable channel
  -> Delivery input LAST CURRENT ONLY with exact pin/EFFECTIVE/READY
  -> [NOT IMPLEMENTED] Live materializer + AlarmLiveProjection
  -> [NOT IMPLEMENTED] operational alarm read model in Command Center Home
```

Exact pin remains:

```text
source_key + result_id + manifest_sha256 + resolution_key
```

Runtime/Delivery do not replace configuration with latest READY and do not reinterpret frozen Rn/Cn from latest Tool Catalog.

READY does not activate Engine.

Web does not process WAL or recalculate alarm priority/routing/cause.

FACTS v2 remains a Runtime output even though Delivery no longer consumes it.

## 4. Web product path now implemented

Current Web application role:

```text
ada-command-center-generic-application==0.1.0
```

Current local route path:

```text
/
  minimal Home

/manager
  Users
  Profiles
  Navigation
  Tool Catalog
  Alarm Configuration
```

This closes the absence of a real Command Center composition root.

It does not close:

```text
operational Alarm visualization
Live
History/Analytics
production identity
durable Users/Profiles/Navigation
Docker/Azure
resource preparation
```

## 5. Local evidence of this close

Administration increment:

```text
HEAD 2ccc2dffd792d55ae68aee3d64ed73eef408bbf8
Configuration Manager 0.1.2
30 PASS
Ruff check PASS
Ruff format check PASS
git diff --check PASS
```

Generic Application increment:

```text
HEAD 736ae9820878a5d8ec7fa7f922ce483be3d3e6b3
Generic Application 0.1.0
6 PASS
Ruff check PASS
Ruff format check PASS
git diff --check PASS
local smoke PASS reported by user
```

This evidence is local and does not substitute CI, Docker or Azure acceptance.

## 6. Full gate still not demonstrated

No single qualified gate currently proves:

```text
infrastructure available
  -> resource preparation
  -> Web/process startup gates
  -> real Tool + confirmed catalog
  -> durable Alarm Source/Projection
  -> current qualification
  -> READY
  -> EFFECTIVE
  -> Engine CURRENT
  -> Delivery CURRENT-only
  -> Live Projection
  -> operational read model rendered in Command Center
```

Do not infer this gate from the presence of the Generic Web application.

## 7. Sequence refinement

The prior statement:

```text
Resource Preparation + startup gate
PLANNED / ÚNICO PRÓXIMO FOCO
```

is SUPERSEDED as sequencing guidance.

Current sequence after the product root was implemented:

```text
Command Center Generic Application       CLOSED / CURRENT
dual-product tooling/distribution         PLANNED / NEXT
Resource Preparation / startup gate       PLANNED / DEFERRED
```

Resource Preparation remains an important open boundary; it is not cancelled.

## 8. Next boundary

**PLANNED / NEXT:** `ADA-COMMAND-CENTER-DUAL-PRODUCT-TOOLING-DISTRIBUTION`.

Objective: audit existing artifact/distribution tooling and make ADA and Command Center explicit product targets without duplicating tooling or importing product-specific capabilities by assumption.

Do not mix into that increment:

```text
Live
Management Projection
History/Analytics
Alarm Engine redesign
Manager branding
production identity implementation
durable Users/Profiles/Navigation implementation
Python migration
```
