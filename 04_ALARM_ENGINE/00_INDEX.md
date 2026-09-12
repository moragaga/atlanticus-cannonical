# Alarm Engine — Index

Estado: **DEEP AUDIT / CANDIDATE**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_DOMAIN_MODEL.md` | AlarmDefinition, Rule, Occurrence, Episode, reappearance. | FROZEN/CURRENT |
| `02_RUNTIME_AND_LIFECYCLE.md` | Cycle, evaluation, priority, lifecycle y commits. | IMPLEMENTED |
| `03_PERSISTENCE_AND_RECOVERY.md` | WAL, durable head, snapshots, recovery. | IMPLEMENTED + VALIDATED |
| `04_CONCURRENCY_LEASES_AND_FENCING.md` | Authority, takeover, stale writers. | IMPLEMENTED + VALIDATED |
| `05_PROJECTION_AND_PUBLICATION.md` | Live vs Management, Runtime/Delivery. | DECISION RECORDED |
| `06_MANAGEMENT.md` | Gestión vs estado físico/live. | CURRENT |
| `07_CONFIGURATION_AND_MATERIALIZATION.md` | SAVE/VALIDATE/EFFECTIVE, LKG, latest valid. | CURRENT SEMANTICS |
| `08_QUALIFICATION_BASELINE.md` | Campaña R3.5 y propiedades demostradas. | CLOSED/GREEN |
| `09_DECISION_INDEX.md` | Genealogía y decisiones individuales. | CANDIDATE |
| `10_OPEN_ITEMS.md` | Reconciliaciones pendientes. | OPEN |
| `11_SOURCE_LEDGER.md` | Inventario de fuentes preservadas y duplicados conocidos. | AUDIT LEDGER |

| `12_COMMAND_CENTER_ANALYTICS_BOUNDARY.md` | Engine → History/Analytics → Command Center Web. | CANDIDATE |
