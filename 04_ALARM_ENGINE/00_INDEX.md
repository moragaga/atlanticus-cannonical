# Alarm Engine — Index

Estado: **CURRENT / B.2 IN PROGRESS — RESOLUTION + RUNTIME/DELIVERY ALIGNMENT + ADOPTION/EFFECTIVE CONTRACT AGREED / NOT YET IMPLEMENTED**

Checkpoint de implementación auditado:

```text
moragaga/atlanticus:main
ebf736a1cf5193a297fbafc55c5c11ca9993f24c
```

Checkpoint canonical base de este delta:

```text
moragaga/atlanticus-cannonical:main
ed49507dbfd808585eb0eb9b89ad1f48b8b3f5a5
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
| `07_CONFIGURATION_AND_MATERIALIZATION.md` | B.2 resolution, findings, Runtime artifact, routing, deactivation/messages, reappearance y visibility. | IN PROGRESS / PROJECT CONTRACT AGREED / NOT IMPLEMENTED |
| `08_QUALIFICATION_BASELINE.md` | Campaña R3.5 y propiedades demostradas. | CLOSED/GREEN |
| `09_DECISION_INDEX.md` | Genealogía y decisiones individuales. | CANDIDATE |
| `10_OPEN_ITEMS.md` | Gaps restantes después del checkpoint B.2. | OPEN / B.2 FOCUSED |
| `11_SOURCE_LEDGER.md` | Inventario de fuentes preservadas y duplicados conocidos. | AUDIT LEDGER |
| `12_COMMAND_CENTER_ANALYTICS_BOUNDARY.md` | Engine → History/Analytics → Command Center Web. | CANDIDATE / FUERA DEL FOCO B.2 |
| `13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md` | Adoption global, Effective Configuration Head, WAL/recovery y exact revision alignment. | PROJECT CONTRACT AGREED / NOT IMPLEMENTED |

No reabrir Management suppression ni Special Condition Runtime reappearance salvo regresión o contradicción nueva entre autoridad e implementación.

Foco único CURRENT:

```text
B.2 — Delivery Configuration Artifact + Live Projection Boundary
```

Ya quedaron acordados, pero todavía no implementados:
- Deactivation + Messages materialization;
- Management Capture provenance y deactivation intent;
- reappearance timer materialization y reconciliation;
- Special Condition qualification;
- TRACE_ONLY exclusivamente como visibility de Delivery;
- `AlarmResolutionKey` explícito;
- `AlarmEffectiveConfigurationHead`;
- Runtime Adoption durable aunque no existan hot-state mutations.

No mezclar todavía con UI final, History/Analytics ni un broad Engine rewrite.
