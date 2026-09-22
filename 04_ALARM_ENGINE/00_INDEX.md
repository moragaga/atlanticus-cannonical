# Alarm Engine — Index

Estado: **CURRENT / DOMAIN EXTRACTION CLOSED / RUNTIME VISIBILITY CLEANUP CLOSED / B.2 CONTRACTS CLOSED / PURE RESOLVER NEXT**

Checkpoint de implementación auditado:

```text
moragaga/atlanticus:main
bc3fffd72afb712d5b5ab84522c379abf2a19642
```

Checkpoint canonical base de este cierre:

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
| `01_DOMAIN_MODEL.md` | Domain/Core boundaries, PlannedAlarm, priority, management, deactivation, visibility y provenance. | CURRENT / IMPLEMENTATION + TARGET GAPS |
| `02_RUNTIME_AND_LIFECYCLE.md` | Cycle, evaluation, lifecycle, management finalization, routing y priority. | IMPLEMENTED / TESTED |
| `03_PERSISTENCE_AND_RECOVERY.md` | WAL, durable head, snapshots, recovery. | IMPLEMENTED + VALIDATED |
| `04_CONCURRENCY_LEASES_AND_FENCING.md` | Authority, takeover, stale writers. | IMPLEMENTED + VALIDATED |
| `05_PROJECTION_AND_PUBLICATION.md` | Live vs Management, Runtime/Delivery. | DECISION RECORDED |
| `06_MANAGEMENT.md` | ManagementEffect, suppression por ranking y reappearance. | CURRENT / IMPLEMENTED / TESTED |
| `07_CONFIGURATION_AND_MATERIALIZATION.md` | B.2 contracts, qualification inputs, resolver boundary y Runtime/Delivery artifacts. | CONTRACTS IMPLEMENTED / PURE RESOLVER NEXT |
| `08_QUALIFICATION_BASELINE.md` | Campaña R3.5 y propiedades demostradas. | CLOSED/GREEN |
| `09_DECISION_INDEX.md` | Genealogía y decisiones individuales. | CANDIDATE |
| `10_OPEN_ITEMS.md` | Gaps posteriores a contratos B.2. | OPEN / PURE RESOLVER FOCUSED |
| `11_SOURCE_LEDGER.md` | Inventario de fuentes preservadas. | AUDIT LEDGER |
| `12_COMMAND_CENTER_ANALYTICS_BOUNDARY.md` | Engine → History/Analytics → Command Center Web. | CANDIDATE / FUERA DEL FOCO |
| `13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md` | Adoption global, Effective Head, WAL/recovery y exact revision alignment. | CONTRACT AGREED / NOT IMPLEMENTED |

## CLOSED

```text
Command Center Alarm Domain Extraction
Alarm Core delivery_enabled / SHADOW root removal
B.2 Materialization Contracts
B.2 Resolver Qualification Input Contracts
```

Physical owner CURRENT:

```text
scopes/ada-command-center/backend/alarms/materialization
```

Future orchestration owner remains:

```text
scopes/ada-command-center/backend/processes/alarms-materialization
```

No reabrir Management suppression, Special Condition Runtime reappearance ni Runtime visibility
salvo regresión o conflicto nuevo demostrado.

## Siguiente foco único

```text
PURE B.2 ALARM CONFIGURATION RESOLVER
```

No mezclar con I/O, stores, job orchestration, Runtime Adoption, Live Delivery,
Management Capture, History/Analytics ni broad Engine rewrite.
