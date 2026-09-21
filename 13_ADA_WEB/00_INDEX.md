# ADA Web — Canonical Index

Estado: **CURRENT**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_CURRENT_BASELINE.md` | Estado Web CURRENT, incluyendo Collector y observability. | CURRENT |
| `02_TESTING_POLICY.md` | Política vigente de tests Web. | CURRENT |
| `03_SHELL_BOUNDARIES.md` | Header/shell operacional ADA vs Manager. | CURRENT |
| `04_QUALIFICATION_HISTORY.md` | Qualification histórica + Collector closure. | CURRENT HISTORY |
| `05_SOURCE_LEDGER.md` | Fuentes utilizadas para reconstrucción. | AUDIT LEDGER |
| `06_INFRASTRUCTURE_STARTUP.md` | Startup/degraded behavior y collector lifecycle. | CURRENT DIRECTION |
| `07_ALARM_MANAGEMENT_FRONTEND.md` | Gestión de alarmas consumiendo authority Backend. | CURRENT DIRECTION |

## KPI Web CURRENT

```text
Registry
core / configuration / projection-local / projection-cosmos

Definition
core / configuration / projection-local / projection-cosmos

Collector
process cache / Cosmos reader / Web integration / browser stores
```

## Web framework CURRENT

```text
WebObservability
→ owned by Atlanticus Web runtime
→ registered in ServiceRegistry
→ consumable by WebModule through WEB_OBSERVABILITY_SERVICE_KEY
```

## Estado

```text
KPI Registry / Definition cutovers     CLOSED / VERIFIED / CURRENT
ADA Web KPI Collector capability       CLOSED / VERIFIED / CURRENT
Collector real Web smoke               CLOSED / VERIFIED / CURRENT
Collector operational application mount PLANNED / NEXT
```
