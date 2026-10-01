# ADA Command Center — Canonical Index

Estado: **CURRENT TRACKING — C1/C2/C4 preserved; Manager 0.3.19 convergence CLOSED locally; final Command Center Generic Application PLANNED / NEXT; Golden Path complete and production acceptance UNVERIFIED.**

## 1. Authority and scope

```text
Last confirmed Atlanticus HEAD in this chat:
moragaga/atlanticus@36361dd570f86e8350ea4a6ee0e09bab351ba171

Command Center Manager convergence delta:
VERIFIED LOCAL / PENDING FINAL GIT HEAD
```

Git remains read-only for this documentation close.

Do not invent a final commit for the local delta. Once integrated, update the checkpoint with the real HEAD.

## 2. Document navigation

| Documento | Propósito / estado |
|---|---|
| `01_PRODUCT_SCOPE.md` | Producto independiente, transversal y distinto de ADA Generic. | CURRENT |
| `02_CURRENT_IMPLEMENTATION.md` | Componentes reales y estado de Manager convergence / Generic Application. | REPLACE WITH THIS CLOSE |
| `03_WEB_APPLICATION.md` | Temporary Manager host and next Generic Application composition boundary. | REPLACE WITH THIS CLOSE |
| `04_CONFIGURATION_SCOPE.md` | Source v3, Tool manifest and Manager workspace convergence. | REPLACE WITH THIS CLOSE |
| `05_TOOL_TO_ALARM_CONFIGURATION.md` | Tool Manifest exacto/Rn-Cn y routing. | PRESERVE |
| `06_ENGINE_AND_PROJECTIONS.md` | Materialization, Engine and Delivery CURRENT-only. | PRESERVE |
| `07_ANALYTICS_AND_STORYTELLING.md` | Analytics independent. | PRESERVE |
| `08_INITIAL_DASHBOARD.md` | Future dashboard concept. | PRESERVE |
| `09_IDENTITY_NAVIGATION_PROFILES.md` | Identity/Users/Profiles/Navigation/Manager boundary after reusable convergence. | REPLACE WITH THIS CLOSE |
| `10_INITIAL_OUT_OF_SCOPE.md` | Product non-goals. | PRESERVE |
| `11_GOLDEN_PATH.md` | End-to-end real gate. | PRESERVE |
| `12_SOURCE_LEDGER.md` | Historical checkpoints. | HISTORICAL / APPEND LATER, DO NOT REWRITE HERE |
| `13_OPEN_ITEMS.md` | Consolidated OPEN and single next focus. | REPLACE WITH THIS CLOSE |
| `14_TOOL_CATALOG.md` | C1 Web Tool Catalog ownership. | PRESERVE |
| `15_ALARM_CONFIGURATION_AUTHORING_MODEL.md` | Source v3 / authoring contract. | PRESERVE |
| `16_ALARM_LIVE_DELIVERY_CONTRACT.md` | Conceptual Live contract. | PRESERVE |
| `17_DOMAIN_OWNERSHIP_AND_MIGRATION.md` | Domain/Web/Backend ownership. | PRESERVE |
| `18_ALARM_AUTHORING_UX_AND_VISUAL_PRESENTATION.md` | UX state. | PRESERVE |

Root authority documents are not modified by this focused close.

## 3. Manager convergence close

Current shared authority:

```text
atlanticus-web-manager==0.3.19
```

Command Center local qualified versions:

```text
ada-command-center-web-alarm-configuration==0.1.1
ada-command-center-web-tool-catalog-manager==0.1.1
ada-command-center-configuration-manager==0.1.1
```

Qualification:

```text
Alarm Configuration       124 PASS
Tool Catalog Manager        9 PASS
Configuration Manager host 28 PASS
Ruff                        PASS
git diff --check            PASS
Manager 0.3.18 rg           EMPTY
```

The temporary host remains temporary.

## 4. Existing product state preserved

C1 Web Tool ownership, C2 Source identity and C4 Delivery CURRENT-only remain under their existing contracts.

Manager convergence does not certify:

```text
Docker
Azure
physical Blob/Cosmos E2E
final Command Center Web application
Live
History/Analytics
```

Existing UX and END_OF_SHIFT open items remain separate.

## 5. Exclusive next boundary

```text
ADA-COMMAND-CENTER-GENERIC-APPLICATION-COMPOSITION
PLANNED / NEXT
```

The next chat must begin with debate/design against authoritative code. It must define the real Command Center composition root before tooling/distribution.

Do not assume Users, Profiles or Navigation are required just because reusable compositions exist.

Do not create a second identity system, Live contract, dashboard, History/Analytics, Resource Preparation or Python migration inside this focus.

After the Generic Application exists and is qualified:

```text
dual-product tooling/distribution
```

may resume.
