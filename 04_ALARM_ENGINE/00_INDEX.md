# Alarm Engine — Index

Estado: **CURRENT / DOMAIN + CORE PREREQUISITES CLOSED / B.2 CONTRACTS CLOSED / PURE RESOLVER NEXT**

Checkpoint de implementación auditado:

```text
moragaga/atlanticus:main
cd08bd8d2c25bd89eb39fa15cbda209c8e9be617
```

Checkpoint canonical base de este cierre:

```text
moragaga/atlanticus-cannonical:main
56943d94889719544f426322ded4a877245dfaee
```

Checkpoint decisions consultado:

```text
moragaga/atlanticus-decisions:main
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

| Archivo | Contenido | Estado |
|---|---|---|
| `01_DOMAIN_MODEL.md` | Domain/Core boundaries, PlannedAlarm, priority, management, deactivation, visibility y provenance. | CURRENT / IMPLEMENTATION + TARGET GAPS |
| `02_RUNTIME_AND_LIFECYCLE.md` | Cycle, evaluation, lifecycle, deactivation/management cascade, routing y priority. | CURRENT / IMPLEMENTED / TESTED |
| `03_PERSISTENCE_AND_RECOVERY.md` | WAL, durable head, snapshots, recovery. | IMPLEMENTED + VALIDATED |
| `04_CONCURRENCY_LEASES_AND_FENCING.md` | Authority, takeover, stale writers. | IMPLEMENTED + VALIDATED |
| `05_PROJECTION_AND_PUBLICATION.md` | Live vs Management, Runtime/Delivery. | DECISION RECORDED |
| `06_MANAGEMENT.md` | ManagementEffect, deactivation barrier, rank suppression y reappearance. | CURRENT / IMPLEMENTED / TESTED |
| `07_CONFIGURATION_AND_MATERIALIZATION.md` | B.2 contracts, qualification inputs, resolver boundary y Runtime/Delivery artifacts. | CONTRACTS IMPLEMENTED / PURE RESOLVER NEXT |
| `08_QUALIFICATION_BASELINE.md` | Campaña R3.5 y propiedades demostradas. | CLOSED/GREEN |
| `09_DECISION_INDEX.md` | Genealogía histórica y refinamientos CURRENT del Project. | CURRENT |
| `10_OPEN_ITEMS.md` | Gaps posteriores a prerequisitos Core/B.2. | OPEN / PURE RESOLVER FOCUSED |
| `11_SOURCE_LEDGER.md` | Inventario de fuentes preservadas. | AUDIT LEDGER |
| `12_COMMAND_CENTER_ANALYTICS_BOUNDARY.md` | Engine → History/Analytics → Command Center Web. | CANDIDATE / FUERA DEL FOCO |
| `13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md` | Adoption global, Effective Head, WAL/recovery y exact revision alignment. | CONTRACT AGREED / NOT IMPLEMENTED |

## CLOSED

```text
Command Center Alarm Domain Extraction
Alarm Core delivery_enabled / SHADOW root removal
B.2 Materialization Contracts
B.2 Resolver Qualification Input Contracts
Deactivation cascade scope
PlannedAlarm.reappearance_after_seconds Runtime contract
```

Deactivation CURRENT:

```text
active DeactivationEffect
-> source DEACTIVATED
-> lower-priority active targets CASCADE_SUPPRESSED
```

Management reappearance no atraviesa una deactivation vigente.

Runtime timer target CURRENT:

```text
PlannedAlarm.reappearance_after_seconds: int | None
```

La conversión Domain minutos -> Runtime segundos todavía pertenece al pure B.2 resolver.

## Siguiente foco único

```text
PURE B.2 ALARM CONFIGURATION RESOLVER
```

No mezclar con I/O, stores, job orchestration, Runtime Adoption, Live Delivery,
Management Capture, History/Analytics, provenance migration ni broad Engine rewrite.
