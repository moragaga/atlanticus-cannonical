# Alarm Engine — Index

Estado: **CURRENT / DOMAIN + CORE + B.2 PURE RESOLVER IMPLEMENTED / MATERIALIZATION PROCESS NEXT**

Checkpoint de implementación auditado:

```text
moragaga/atlanticus:main
9398786ae9af7c00de1bcca9d7a311fe9ef2155f
```

Checkpoint canonical inspeccionado antes de este reemplazo:

```text
moragaga/atlanticus-cannonical:main
7fea2819aa4c22f9d7494cbe79ab8740ee3f4366
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
| `07_CONFIGURATION_AND_MATERIALIZATION.md` | B.2 contracts, pure resolver, qualification inputs y artifacts Runtime/Delivery. | CURRENT / RESOLVER IMPLEMENTED |
| `08_QUALIFICATION_BASELINE.md` | Campaña R3.5 y qualification observada del resolver B.2. | CURRENT / EVIDENCE |
| `09_DECISION_INDEX.md` | Genealogía histórica y refinamientos CURRENT del Project. | CURRENT |
| `10_OPEN_ITEMS.md` | Gaps posteriores al resolver B.2. | OPEN / MATERIALIZATION PROCESS FOCUSED |
| `11_SOURCE_LEDGER.md` | Inventario de fuentes preservadas. | AUDIT LEDGER |
| `12_COMMAND_CENTER_ANALYTICS_BOUNDARY.md` | Engine → History/Analytics → Command Center Web. | CANDIDATE / FUERA DEL FOCO |
| `13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md` | Adoption global, Effective Head, WAL/recovery y exact revision alignment. | CONTRACT AGREED / NOT IMPLEMENTED |

## CLOSED / CURRENT

```text
Command Center Alarm Domain Extraction
Alarm Core delivery_enabled / SHADOW root removal
B.2 Materialization Contracts
B.2 Resolver Qualification Input Contracts
Deactivation cascade scope
PlannedAlarm.reappearance_after_seconds Runtime contract
Pure B.2 Alarm Configuration Resolver implementation
```

Pure resolver CURRENT:

```text
AlarmConfiguration
+ alarm revision
+ exact Confirmed Tool Catalog
+ Tool reconciliation qualification
+ Evaluator qualification
-> AlarmConfigurationResolution
```

Sin I/O ni orchestration.

Qualification observada:
- 31 tests PASS;
- `ruff check` PASS;
- salida final posterior de `ruff format --check` no preservada en este cierre.

## Siguiente foco único

```text
B.2 MATERIALIZATION PROCESS
```

Gate de entrada: confirmar formatter GREEN una vez y continuar; no reabrir semántica B.2 si no
aparece un conflicto real.

No mezclar con Runtime Adoption, Live Delivery, Management Capture, History/Analytics,
provenance migration ni broad Engine rewrite.
