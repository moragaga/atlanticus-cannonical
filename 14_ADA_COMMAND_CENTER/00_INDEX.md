# ADA Command Center — Canonical Index

Estado: **CURRENT / DOMAIN OWNERSHIP CLOSED IN DESIGN — ALARM DOMAIN EXTRACTION NEXT**

Checkpoint de implementación auditado:

```text
moragaga/atlanticus:main
ebf736a1cf5193a297fbafc55c5c11ca9993f24c
```

Checkpoint canonical base de este delta:

```text
moragaga/atlanticus-cannonical:main
b85e6b2b27e39d0531b95f3fbe97ef6c8fd06949
```

| Archivo | Contenido | Estado |
|---|---|---|
| `01_PRODUCT_SCOPE.md` | Propósito y ownership de Command Center. | CURRENT DIRECTION |
| `02_CURRENT_IMPLEMENTATION.md` | Qué existe hoy en `main`. | CURRENT; revisar sólo cuando cambie inventario global |
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
| `13_OPEN_ITEMS.md` | Gaps posteriores al cierre de ownership y Live Delivery. | OPEN / IMPLEMENTATION NEXT |
| `14_TOOL_CATALOG.md` | Consolidador durable read-only de Tools. | CURRENT V1 / CLOSED |
| `15_ALARM_CONFIGURATION_AUTHORING_MODEL.md` | Authoring + validation + materialization/adoption contract. | CURRENT + PROJECT CONTRACT AGREED / NOT IMPLEMENTED |
| `16_ALARM_LIVE_DELIVERY_CONTRACT.md` | Delivery artifact, Engine current-state output, cause materialization, Live publication y Management round-trip. | PROJECT CONTRACT AGREED / NOT IMPLEMENTED |
| `17_DOMAIN_OWNERSHIP_AND_MIGRATION.md` | `domain/backend/web`, ownership transversal de Alarm Configuration, B.2 package boundary y orden de migración. | PROJECT CONTRACT AGREED / MIGRATION NOT IMPLEMENTED |

Foco único CURRENT:

```text
Command Center — Alarm Domain Extraction
```

Target arquitectónico previo al B.2 funcional:

```text
scopes/ada-command-center/domain/alarms
```

Web Configuration y Backend Alarm Engine consumen el mismo dominio; B.2 pure materialization queda target en `backend/alarms/materialization` y su proceso en `backend/processes/alarms-materialization`.

No mezclar el primer incremento de migración con B.2 funcional, Runtime Adoption, Live Delivery, UI final ni History/Analytics.
