# Alarm Engine — Index

Estado: **CURRENT / PURE B.2 RESOLVER + EXACT ALARM/TOOL SNAPSHOT CURRENT / OPERATIONAL PROJECTION NEXT**

Checkpoint de implementación:

```text
moragaga/atlanticus:main
880cb692054c2cd78cdc29cf62ab7b16bbd2c3d6
```

Último commit semántico de este cierre:

```text
d2a5e14822d3711e64668b8e70cfa15d7ddae2f0
```

| Archivo | Contenido | Estado |
|---|---|---|
| `01_DOMAIN_MODEL.md` | Domain/Core boundaries y Runtime model. | CURRENT |
| `02_RUNTIME_AND_LIFECYCLE.md` | Cycle, lifecycle, routing, priority. | CURRENT / IMPLEMENTED |
| `03_PERSISTENCE_AND_RECOVERY.md` | WAL, recovery, snapshots. | CURRENT / IMPLEMENTED |
| `04_CONCURRENCY_LEASES_AND_FENCING.md` | Authority y stale writers. | CURRENT / IMPLEMENTED |
| `05_PROJECTION_AND_PUBLICATION.md` | Source/base projection vs Runtime/Delivery/Live/Management. | CURRENT / UPDATED |
| `06_MANAGEMENT.md` | Management, deactivation, suppression, reappearance. | CURRENT |
| `07_CONFIGURATION_AND_MATERIALIZATION.md` | B.2 resolver + exact Tool dependency evidence. | CURRENT / UPDATED |
| `08_QUALIFICATION_BASELINE.md` | Qualification histórica. | EVIDENCE |
| `09_DECISION_INDEX.md` | Genealogía y refinamientos. | CURRENT / UPDATED |
| `10_OPEN_ITEMS.md` | Gaps posteriores al snapshot v3. | OPEN / COSMOS PROJECTION NEXT |
| `11_SOURCE_LEDGER.md` | Fuentes y checkpoints. | AUDIT LEDGER / UPDATED |
| `12_COMMAND_CENTER_ANALYTICS_BOUNDARY.md` | History/Analytics boundary. | SEPARATE |
| `13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md` | Effective Head y adoption. | CONTRACT AGREED / NOT IMPLEMENTED |

## CLOSED / CURRENT acumulado

```text
Alarm Domain extraction
Alarm Core visibility cleanup
B.2 Materialization contracts
B.2 qualification input contracts
Pure B.2 resolver
Command Center Tools domain contract
ToolDependencyManifest
AlarmConfigurationSnapshot v3
Alarm/Tool validate-publish correlation
```

## Frontera exacta hacia B.2

```text
Alarm Source release Rn
    |
    v
AlarmConfigurationSnapshot
    configuration
    ToolDependencyManifest(Cn)
    |
    v
B.2 resolver
```

No usar `latest Tool Catalog` para reinterpretar una release Alarm ya publicada.

## Siguiente foco único

```text
Alarm Configuration operational Projection to Cosmos
```
