# ADA Command Center — Canonical Index

Estado: **CURRENT / B.2 IN PROGRESS — RESOLUTION + RUNTIME ARTIFACT + ROUTING CONTRACT AGREED / NOT YET IMPLEMENTED**

Checkpoint de implementación auditado:

```text
moragaga/atlanticus:main
ebf736a1cf5193a297fbafc55c5c11ca9993f24c
```

Checkpoint canonical base de este delta:

```text
moragaga/atlanticus-cannonical:main
85a12f5ccdf0b5d03992396b13d4b1914b7062a3
```

| Archivo | Contenido | Estado |
|---|---|---|
| `01_PRODUCT_SCOPE.md` | Propósito y ownership de Command Center. | CURRENT DIRECTION |
| `02_CURRENT_IMPLEMENTATION.md` | Qué existe hoy en `main`. | CURRENT; revisar sólo cuando cambie inventario global |
| `03_WEB_APPLICATION.md` | Web propia, shell e identidad. | PARTIALLY IMPLEMENTED |
| `04_CONFIGURATION_SCOPE.md` | Alarm Configuration, Manager y límites. | CURRENT |
| `05_TOOL_TO_ALARM_CONFIGURATION.md` | Tool topology → Tool Catalog → authoring references → B.2. | CURRENT / B.2 INPUT CONTRACT PARTIALLY OPEN |
| `06_ENGINE_AND_PROJECTIONS.md` | Engine hot state, Runtime/Delivery, Live, Management e History/Analytics. | CURRENT / B.2 BOUNDARY REFINED |
| `07_ANALYTICS_AND_STORYTELLING.md` | Modelo de análisis y conclusiones trazables. | PRODUCT DIRECTION |
| `08_INITIAL_DASHBOARD.md` | Preguntas y capacidades del dashboard inicial. | PROPOSED |
| `09_IDENTITY_NAVIGATION_PROFILES.md` | Entra ID, Navigation y Profiles. | CURRENT DIRECTION |
| `10_INITIAL_OUT_OF_SCOPE.md` | Qué no entra inicialmente. | CURRENT DIRECTION |
| `11_GOLDEN_PATH.md` | Vertical integrada. | PARTIALLY IMPLEMENTED |
| `12_SOURCE_LEDGER.md` | Fuentes auditadas y trazabilidad. | AUDIT LEDGER |
| `13_OPEN_ITEMS.md` | Contratos pendientes después del cierre parcial de B.2. | OPEN / B.2 IN PROGRESS |
| `14_TOOL_CATALOG.md` | Consolidador durable read-only de Tools. | CURRENT V1 / CLOSED |
| `15_ALARM_CONFIGURATION_AUTHORING_MODEL.md` | Authoring + validation layers + B.2 resolution/routing contract. | CURRENT + PROJECT CONTRACT AGREED / NOT IMPLEMENTED |

Foco único CURRENT:

```text
B.2 — Alarm Configuration -> Runtime/Delivery Configuration Materialization
```

Siguiente subfoco:

```text
Deactivation + Messages materialization
```

No mezclar todavía con Delivery execution final, UI final, History/Analytics ni nuevos cambios del Engine.
