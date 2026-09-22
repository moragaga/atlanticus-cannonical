# Alarm Engine — Index

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

Checkpoint decisions consultado:

```text
moragaga/atlanticus-decisions:main
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

| Archivo | Contenido | Estado |
|---|---|---|
| `01_DOMAIN_MODEL.md` | AlarmDefinition, PlannedAlarm, evaluator boundary, Rule, Occurrence, Episode, ranking y reappearance. | CURRENT / IMPLEMENTED; B.2 BOUNDARY REFINED |
| `02_RUNTIME_AND_LIFECYCLE.md` | Cycle, evaluation, lifecycle, management finalization, routing y priority. | IMPLEMENTED / TESTED |
| `03_PERSISTENCE_AND_RECOVERY.md` | WAL, durable head, snapshots, recovery. | IMPLEMENTED + VALIDATED |
| `04_CONCURRENCY_LEASES_AND_FENCING.md` | Authority, takeover, stale writers. | IMPLEMENTED + VALIDATED |
| `05_PROJECTION_AND_PUBLICATION.md` | Live vs Management, Runtime/Delivery. | DECISION RECORDED |
| `06_MANAGEMENT.md` | ManagementEffect, suppression por ranking y reappearance. | CURRENT / IMPLEMENTED / TESTED |
| `07_CONFIGURATION_AND_MATERIALIZATION.md` | B.2 resolution, findings, READY/BLOCKED, Runtime artifact y routing materialization. | IN PROGRESS / CONTRACT PARTIALLY FROZEN / NOT IMPLEMENTED |
| `08_QUALIFICATION_BASELINE.md` | Campaña R3.5 y propiedades demostradas. | CLOSED/GREEN |
| `09_DECISION_INDEX.md` | Genealogía y decisiones individuales. | CANDIDATE |
| `10_OPEN_ITEMS.md` | Gaps restantes después del cierre parcial de B.2. | OPEN / B.2 FOCUSED |
| `11_SOURCE_LEDGER.md` | Inventario de fuentes preservadas y duplicados conocidos. | AUDIT LEDGER |
| `12_COMMAND_CENTER_ANALYTICS_BOUNDARY.md` | Engine → History/Analytics → Command Center Web. | CANDIDATE / FUERA DEL FOCO B.2 |

No reabrir Management suppression ni Special Condition Runtime reappearance salvo regresión o contradicción nueva entre autoridad e implementación.

Foco único CURRENT:

```text
B.2 — Alarm Configuration -> Runtime/Delivery Configuration Materialization
```

No mezclar todavía con Deactivation/Message resolution no acordada, Delivery execution final, History/Analytics ni nuevos cambios del Engine.
