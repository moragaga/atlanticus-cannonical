# Alarm Engine — Index

Estado: **CURRENT / MANAGEMENT + SPECIAL-CONDITION RUNTIME MILESTONE CLOSED / B.2 NEXT**

Checkpoint de implementación auditado:

```text
moragaga/atlanticus:main
ebf736a1cf5193a297fbafc55c5c11ca9993f24c
```

Checkpoint canonical previo a esta actualización:

```text
moragaga/atlanticus-cannonical:main
d8b69e914cbdd49c9f53f0c27d90eb38a0c1b86f
```

| Archivo | Contenido | Estado |
|---|---|---|
| `01_DOMAIN_MODEL.md` | AlarmDefinition, PlannedAlarm, Rule, Occurrence, Episode, ranking y reappearance. | CURRENT / IMPLEMENTED; B.1 RECONCILIATION OPEN |
| `02_RUNTIME_AND_LIFECYCLE.md` | Cycle, evaluation, lifecycle, management finalization, routing y priority. | IMPLEMENTED / TESTED |
| `03_PERSISTENCE_AND_RECOVERY.md` | WAL, durable head, snapshots, recovery. | IMPLEMENTED + VALIDATED |
| `04_CONCURRENCY_LEASES_AND_FENCING.md` | Authority, takeover, stale writers. | IMPLEMENTED + VALIDATED |
| `05_PROJECTION_AND_PUBLICATION.md` | Live vs Management, Runtime/Delivery. | DECISION RECORDED |
| `06_MANAGEMENT.md` | ManagementEffect, suppression por ranking y reappearance. | CURRENT / IMPLEMENTED / TESTED |
| `07_CONFIGURATION_AND_MATERIALIZATION.md` | Source válida, resolución externa y futura materialización B.2. | CURRENT SEMANTICS / B.2 NEXT |
| `08_QUALIFICATION_BASELINE.md` | Campaña R3.5 y propiedades demostradas. | CLOSED/GREEN |
| `09_DECISION_INDEX.md` | Genealogía y decisiones individuales. | CANDIDATE |
| `10_OPEN_ITEMS.md` | Reconciliaciones pendientes después del cierre Runtime. | OPEN / B.2 FOCUSED |
| `11_SOURCE_LEDGER.md` | Inventario de fuentes preservadas y duplicados conocidos. | AUDIT LEDGER |
| `12_COMMAND_CENTER_ANALYTICS_BOUNDARY.md` | Engine → History/Analytics → Command Center Web. | CANDIDATE |

No reabrir Management suppression ni Special Condition Runtime reappearance salvo regresión o contradicción nueva entre autoridad e implementación.
