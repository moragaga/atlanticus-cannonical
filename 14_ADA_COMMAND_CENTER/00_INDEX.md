# ADA Command Center — Canonical Index

Estado: **CURRENT TRACKING — C1/C2/C4 preserved; Manager 0.3.19 convergence CLOSED; Command Center administration composition CLOSED; Generic Application 0.1.0 CURRENT / VERIFIED LOCAL; dual-product tooling/distribution PLANNED / NEXT; Golden Path production acceptance UNVERIFIED.**

## 1. Authority and scope

```text
Implementation CURRENT
moragaga/atlanticus:main@736ae9820878a5d8ec7fa7f922ce483be3d3e6b3

Command Center administration integration
moragaga/atlanticus@2ccc2dffd792d55ae68aee3d64ed73eef408bbf8

Command Center Generic Application
moragaga/atlanticus@736ae9820878a5d8ec7fa7f922ce483be3d3e6b3

Decisions inspected
moragaga/atlanticus-decisions:main@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e

Canonical base before this replacement
moragaga/atlanticus-cannonical:main@226bcded9eb6970292c7722b23b595e93b4a9cff
```

Git remains read-only for this documentation close.

Implementation is the authority for what exists. Decisions remain authority for explicit frozen intent. If they conflict, preserve the conflict instead of reconciling it silently.

## 2. Document navigation

| Documento | Propósito / estado |
|---|---|
| `01_PRODUCT_SCOPE.md` | Producto independiente, transversal y distinto de ADA Generic. | PRESERVE / CURRENT |
| `02_CURRENT_IMPLEMENTATION.md` | Componentes reales, Generic Application y estado actual. | REPLACE WITH THIS CLOSE |
| `03_WEB_APPLICATION.md` | Product composition root, standalone qualification app and runtime boundary. | REPLACE WITH THIS CLOSE |
| `04_CONFIGURATION_SCOPE.md` | Source v3, Tool manifest, Manager workspace and administration composition. | REPLACE WITH THIS CLOSE |
| `05_TOOL_TO_ALARM_CONFIGURATION.md` | Tool Manifest exacto/Rn-Cn y routing. | PRESERVE |
| `06_ENGINE_AND_PROJECTIONS.md` | Materialization, Engine and Delivery CURRENT-only. | PRESERVE |
| `07_ANALYTICS_AND_STORYTELLING.md` | Analytics independent. | PRESERVE |
| `08_INITIAL_DASHBOARD.md` | Future dashboard concept. | PRESERVE |
| `09_IDENTITY_NAVIGATION_PROFILES.md` | Identity/Users/Profiles/Navigation/Manager current integration boundary. | REPLACE WITH THIS CLOSE |
| `10_INITIAL_OUT_OF_SCOPE.md` | Product non-goals. | PRESERVE |
| `11_GOLDEN_PATH.md` | End-to-end real gate and current sequence. | REPLACE WITH THIS CLOSE |
| `12_SOURCE_LEDGER.md` | Historical checkpoints plus 2026-10-01 composition close. | REPLACE / APPEND HISTORY |
| `13_OPEN_ITEMS.md` | Consolidated OPEN and single next focus. | REPLACE WITH THIS CLOSE |
| `14_TOOL_CATALOG.md` | C1 Web Tool Catalog ownership. | PRESERVE |
| `15_ALARM_CONFIGURATION_AUTHORING_MODEL.md` | Source v3 / authoring contract. | PRESERVE |
| `16_ALARM_LIVE_DELIVERY_CONTRACT.md` | Conceptual Live contract. | PRESERVE |
| `17_DOMAIN_OWNERSHIP_AND_MIGRATION.md` | Domain/Web/Backend ownership. | PRESERVE |
| `18_ALARM_AUTHORING_UX_AND_VISUAL_PRESENTATION.md` | UX state. | PRESERVE |

Root authority documents are not modified by this focused close.

## 3. Web composition state

Current shared Manager authority:

```text
atlanticus-web-manager==0.3.19
```

Command Center CURRENT packages relevant to this close:

```text
ada-command-center-web-alarm-configuration==0.1.1
ada-command-center-web-tool-catalog-manager==0.1.1
ada-command-center-configuration-manager==0.1.2
ada-command-center-generic-application==0.1.0
```

`ada-command-center-configuration-manager` is a separate development/testing/qualification application. It is not the product application and is not executed or absorbed as a standalone host by Generic.

`ada-command-center-generic-application` is the real Command Center Web composition root. It reuses the configuration-manager package composition/contracts, analogously to the established ADA application-role pattern, without copying ADA product-specific runtime behavior.

## 4. Local qualification of this close

Reported and observed in the integration workflow:

```text
Command Center Configuration Manager 0.1.2
pytest                      30 PASS
Ruff check                  PASS
Ruff format check           PASS
git diff --check            PASS

Command Center Generic Application 0.1.0
pytest                       6 PASS
Ruff check                  PASS
Ruff format check           PASS
git diff --check            PASS
local application smoke     PASS reported by user
```

This does not certify CI, Docker, Azure, durable identity, physical Blob/Cosmos equivalence or production deployment.

## 5. Current product composition

```text
ada-command-center-generic-application
│
├── Home /
├── Identity
│   └── local provider in launcher 0.1.0
├── projected operational Navigation
└── Manager /manager
    ├── Users
    ├── Profiles
    ├── Navigation
    ├── Tool Catalog
    └── Alarm Configuration
```

Explicit exclusions preserved:

```text
ADA Access
ADA KPI Registry / Definition
ADA Collector
ADA Tool operational projection
ADA-specific shell
```

Live, Management Projection, History/Analytics and Alarm Engine redesign remain separate fronts.

## 6. Exclusive next boundary

```text
ADA-COMMAND-CENTER-DUAL-PRODUCT-TOOLING-DISTRIBUTION
PLANNED / NEXT
```

Reason: the product target that previously blocked dual-product tooling now exists and is locally qualified.

The next focus must audit existing artifact/distribution tooling before modifying it. It must not assume the current tooling is ADA-neutral and must not create parallel tooling if the existing implementation can be parameterized or composed.

Resource Preparation/startup gate remains PLANNED / DEFERRED and is not the next focus in this sequence.
