# Alarm Engine — Index

Estado: **CURRENT / ROUTING STRICT IMPLEMENTED / MATERIALIZATION JOB PLANNED**

Checkpoint auditado:

```text
moragaga/atlanticus@411aea44ac60c09d2b07ce41d34c3f378788b97b
moragaga/atlanticus-cannonical@83cd871c8418e37d2c29dff30e2ea5ef54bda4a0 (antes de estos reemplazos)
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

| Archivo | Propósito | Estado para este corte |
|---|---|---|
| `01_DOMAIN_MODEL.md` | Domain/Core boundaries y Runtime model. | CURRENT; no modificado aquí. |
| `02_RUNTIME_AND_LIFECYCLE.md` | Lifecycle, routing de ocurrencias, priority. | CURRENT; no modificado aquí. |
| `03_PERSISTENCE_AND_RECOVERY.md` | WAL, recovery, snapshots. | CURRENT; no modificado aquí. |
| `04_CONCURRENCY_LEASES_AND_FENCING.md` | Stale writers/fencing. | CURRENT; no modificado aquí. |
| `05_PROJECTION_AND_PUBLICATION.md` | Base/Live/Management boundaries. | CURRENT; revisar sólo ante cambios del siguiente frente. |
| `06_MANAGEMENT.md` | Management, suppression y reappearance. | CURRENT; no modificado aquí. |
| `07_CONFIGURATION_AND_MATERIALIZATION.md` | Source v3, stores, pure B.2, strict routing, siguiente job. | CURRENT / UPDATED. |
| `08_QUALIFICATION_BASELINE.md` | Qualification histórica + evidencia nueva acotada. | EVIDENCE / UPDATED. |
| `09_DECISION_INDEX.md` | Decisiones históricas y refinamiento estricto. | CURRENT / UPDATED. |
| `10_OPEN_ITEMS.md` | Fronteras pendientes sin mezclar incrementos. | OPEN / MATERIALIZATION NEXT. |
| `11_SOURCE_LEDGER.md` | SHA, paths y evidencia exacta de las suites. | AUDIT LEDGER / UPDATED. |
| `12_COMMAND_CENTER_ANALYTICS_BOUNDARY.md` | Separación Engine/Analytics. | SEPARATE; no modificado aquí. |
| `13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md` | `READY != EFFECTIVE`; adoption posterior. | CONTRACT AGREED / NOT IMPLEMENTED. |

## CURRENT acumulado

```text
Alarm Domain + Core
Pure B.2 resolver + Tool/Evaluator qualification input contracts
ToolDependencyManifest + AlarmConfigurationSnapshot v3
Source/Base Projection + Local/Cosmos projection stores/composition
Validate/Publish exact Alarm/Tool correlation
Strict next-level routing policy in Domain + B.2
Matching guided routing options in Web
```

El flujo operativo verificado aún no incluye un proceso de Materialization que consuma Cosmos y persista/exponga outputs a Runtime. Un store disponible no equivale a un job desplegado.

```text
Alarm source release Rn
 -> AlarmConfigurationSnapshot(configuration, ToolDependencyManifest(Cn))
 -> operational ProjectionRecord[AlarmConfigurationSnapshot]
 -> PLANNED backend materialization job
 -> CURRENT pure B.2 resolver
 -> PLANNED persisted Runtime/Delivery/findings
 -> LATER Runtime Adoption / Effective Head
```

Siguiente foco único: **materialization job y su frontera de entrada/salida existente**, sin ampliar a Adoption/Live/Analytics.
