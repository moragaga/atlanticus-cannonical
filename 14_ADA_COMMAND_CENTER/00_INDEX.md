# ADA Command Center — Canonical Index

Estado: **CURRENT / ALARM DOMAIN EXTRACTION CLOSED / B.2 MATERIALIZATION CONTRACTS NEXT**

Checkpoint de implementación auditado:

```text
moragaga/atlanticus:main
7a8c36a29860c8f010c3fe5b840c5f5af4d87d0f
```

Checkpoint canonical base de este cierre:

```text
moragaga/atlanticus-cannonical:main
8f915fa75b6f4eaaa80fb003b290c613aa9ad735
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
| `05_TOOL_TO_ALARM_CONFIGURATION.md` | Tool topology → Tool Catalog → authoring references → B.2. | CURRENT / B.2 INPUT CONTRACT PARTIALLY OPEN |
| `06_ENGINE_AND_PROJECTIONS.md` | Engine hot state, Runtime/Delivery, Effective Head, Live y Management. | CURRENT + PROJECT CONTRACT REFINED |
| `07_ANALYTICS_AND_STORYTELLING.md` | Modelo de análisis y conclusiones trazables. | PRODUCT DIRECTION |
| `08_INITIAL_DASHBOARD.md` | Preguntas y capacidades del dashboard inicial. | PROPOSED |
| `09_IDENTITY_NAVIGATION_PROFILES.md` | Entra ID, Navigation y Profiles. | CURRENT DIRECTION |
| `10_INITIAL_OUT_OF_SCOPE.md` | Qué no entra inicialmente. | CURRENT DIRECTION |
| `11_GOLDEN_PATH.md` | Vertical integrada. | PARTIALLY IMPLEMENTED |
| `12_SOURCE_LEDGER.md` | Fuentes auditadas y trazabilidad. | AUDIT LEDGER |
| `13_OPEN_ITEMS.md` | Gaps posteriores a Domain Extraction. | OPEN / B.2 IMPLEMENTATION NEXT |
| `14_TOOL_CATALOG.md` | Consolidador durable read-only de Tools. | CURRENT V1 / CLOSED |
| `15_ALARM_CONFIGURATION_AUTHORING_MODEL.md` | Authoring + validation + materialization/adoption contract. | CURRENT + PROJECT CONTRACT AGREED / NOT IMPLEMENTED |
| `16_ALARM_LIVE_DELIVERY_CONTRACT.md` | Delivery artifact, Engine current-state output, cause materialization, Live publication y Management round-trip. | PROJECT CONTRACT AGREED / NOT IMPLEMENTED |
| `17_DOMAIN_OWNERSHIP_AND_MIGRATION.md` | `domain/backend/web`, ownership transversal de Alarm Configuration, B.2 package boundary y migración. | CURRENT / IMPLEMENTED / CLOSED |

## Cerrado en este hito

```text
Command Center — Alarm Domain Extraction
CLOSED
```

Authority CURRENT:

```text
scopes/ada-command-center/domain/alarms
ada_command_center.domain.alarms
```

Web Configuration y Backend Alarm Engine consumen el mismo authored domain.

Las antiguas autoridades:
- `backend/alarms/core/definition.py`;
- `web/alarms/configuration/models.py`;

ya no existen en `main`.

No se conservaron aliases legacy para mantener esas autoridades anteriores.

## Foco único siguiente

```text
B.2 — Materialization Contracts
```

Target:

```text
scopes/ada-command-center/backend/alarms/materialization
```

Primer incremento:
- contratos/value objects puros ya acordados;
- sin I/O;
- sin stores;
- sin resolver completo;
- sin process orchestration;
- sin Runtime Adoption;
- sin Live Delivery;
- sin Management Capture.
