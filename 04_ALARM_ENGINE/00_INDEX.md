# Alarm Engine — Index

Estado: **CURRENT / B.2 DESIGN CLOSED THROUGH LIVE DELIVERY + OWNERSHIP BOUNDARY AGREED / IMPLEMENTATION PENDING**

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

Checkpoint decisions consultado:

```text
moragaga/atlanticus-decisions:main
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

| Archivo | Contenido | Estado |
|---|---|---|
| `01_DOMAIN_MODEL.md` | AlarmDefinition, PlannedAlarm, evaluator boundary, priority, management, deactivation, visibility y reappearance. | CURRENT / IMPLEMENTED + TARGET CONTRACT REFINED |
| `02_RUNTIME_AND_LIFECYCLE.md` | Cycle, evaluation, lifecycle, management finalization, routing y priority. | IMPLEMENTED / TESTED |
| `03_PERSISTENCE_AND_RECOVERY.md` | WAL, durable head, snapshots, recovery. | IMPLEMENTED + VALIDATED |
| `04_CONCURRENCY_LEASES_AND_FENCING.md` | Authority, takeover, stale writers. | IMPLEMENTED + VALIDATED |
| `05_PROJECTION_AND_PUBLICATION.md` | Live vs Management, Runtime/Delivery. | DECISION RECORDED |
| `06_MANAGEMENT.md` | ManagementEffect, suppression por ranking y reappearance. | CURRENT / IMPLEMENTED / TESTED |
| `07_CONFIGURATION_AND_MATERIALIZATION.md` | B.2 resolution, Runtime/Delivery artifacts, routing, deactivation/messages, reappearance, visibility y Live Delivery boundary. | PROJECT CONTRACT AGREED / NOT IMPLEMENTED |
| `08_QUALIFICATION_BASELINE.md` | Campaña R3.5 y propiedades demostradas. | CLOSED/GREEN |
| `09_DECISION_INDEX.md` | Genealogía y decisiones individuales. | CANDIDATE |
| `10_OPEN_ITEMS.md` | Gaps de implementación después del cierre conceptual B.2. | OPEN / IMPLEMENTATION FOCUSED |
| `11_SOURCE_LEDGER.md` | Inventario de fuentes preservadas y duplicados conocidos. | AUDIT LEDGER |
| `12_COMMAND_CENTER_ANALYTICS_BOUNDARY.md` | Engine → History/Analytics → Command Center Web. | CANDIDATE / FUERA DEL FOCO B.2 |
| `13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md` | Adoption global, Effective Configuration Head, WAL/recovery y exact revision alignment. | PROJECT CONTRACT AGREED / NOT IMPLEMENTED |

No reabrir Management suppression ni Special Condition Runtime reappearance salvo regresión o contradicción nueva entre autoridad e implementación.

Prerequisito arquitectónico acordado antes de implementar B.2:

```text
Command Center Alarm Domain Extraction
-> scopes/ada-command-center/domain/alarms
```

El modelo authoring compartido deja de ser propiedad física de Engine Core/Web Configuration. Ver `../14_ADA_COMMAND_CENTER/17_DOMAIN_OWNERSHIP_AND_MIGRATION.md`.

Después del Domain extraction, B.2 pure resolution tiene target:

```text
backend/alarms/materialization
```

y la orquestación operacional:

```text
backend/processes/alarms-materialization
```

No mezclar la migración de dominio con implementación B.2 funcional, UI final, History/Analytics ni broad Engine rewrite.
