# ADA Command Center — Canonical Index

Estado: **CURRENT / ALARM DOMAIN + RUNTIME VISIBILITY + B.2 CONTRACTS CLOSED / PURE RESOLVER NEXT**

Checkpoint de implementación auditado:

```text
moragaga/atlanticus:main
bc3fffd72afb712d5b5ab84522c379abf2a19642
```

Checkpoint canonical base:

```text
moragaga/atlanticus-cannonical:main
2d8cbc33b7776e057e4f7d82def318d5eaf8f336
```

Checkpoint decisions consultado:

```text
moragaga/atlanticus-decisions:main
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

| Archivo | Contenido | Estado |
|---|---|---|
| `01_PRODUCT_SCOPE.md` | Propósito y ownership de Command Center. | CURRENT DIRECTION |
| `02_CURRENT_IMPLEMENTATION.md` | Qué existe hoy en `main`. | CURRENT / UPDATED |
| `03_WEB_APPLICATION.md` | Web propia, shell e identidad. | PARTIALLY IMPLEMENTED |
| `04_CONFIGURATION_SCOPE.md` | Alarm Configuration, Manager y límites. | CURRENT |
| `05_TOOL_TO_ALARM_CONFIGURATION.md` | Tool topology → Tool Catalog → authoring references → B.2. | CURRENT / PRODUCER QUALIFICATION OPEN |
| `06_ENGINE_AND_PROJECTIONS.md` | Engine hot state, Runtime/Delivery, Effective Head, Live y Management. | CURRENT + PROJECT CONTRACT |
| `07_ANALYTICS_AND_STORYTELLING.md` | Modelo de análisis y conclusiones trazables. | PRODUCT DIRECTION |
| `08_INITIAL_DASHBOARD.md` | Preguntas y capacidades del dashboard inicial. | PROPOSED |
| `09_IDENTITY_NAVIGATION_PROFILES.md` | Entra ID, Navigation y Profiles. | CURRENT DIRECTION |
| `10_INITIAL_OUT_OF_SCOPE.md` | Qué no entra inicialmente. | CURRENT DIRECTION |
| `11_GOLDEN_PATH.md` | Vertical integrada. | PARTIALLY IMPLEMENTED |
| `12_SOURCE_LEDGER.md` | Fuentes auditadas. | AUDIT LEDGER |
| `13_OPEN_ITEMS.md` | Gaps después de B.2 contracts. | OPEN / PURE RESOLVER NEXT |
| `14_TOOL_CATALOG.md` | Consolidador durable read-only de Tools. | CURRENT V1 / CLOSED |
| `15_ALARM_CONFIGURATION_AUTHORING_MODEL.md` | Authoring + validation + B.2 boundary. | CURRENT + TARGET CONTRACT |
| `16_ALARM_LIVE_DELIVERY_CONTRACT.md` | Delivery, Engine current state, cause, publication, Management round-trip. | PROJECT CONTRACT / NOT IMPLEMENTED |
| `17_DOMAIN_OWNERSHIP_AND_MIGRATION.md` | Domain/Core/Materialization ownership. | CURRENT / UPDATED |

## CLOSED acumulado

```text
Alarm Domain Extraction
Alarm Core delivery_enabled / SHADOW root removal
B.2 Materialization Contracts
B.2 Resolver Qualification Input Contracts
```

Materialization owner CURRENT:

```text
scopes/ada-command-center/backend/alarms/materialization
```

## Foco único siguiente

```text
PURE B.2 ALARM CONFIGURATION RESOLVER
```

Sin I/O, stores, process orchestration, Runtime Adoption, Live Delivery ni Management Capture.
