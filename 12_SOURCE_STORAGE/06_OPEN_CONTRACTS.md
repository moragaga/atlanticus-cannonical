# Source Storage — Open Contracts

Estado: **IN PROGRESS — POST MANAGER GENERIC CUTOVER**

## Core Source — CLOSED

Congelado:

1. `SourceKey`.
2. `SourceReleaseId`.
3. `SourceReleaseRef`.
4. release id separado de content hash.
5. immutable releases.
6. manifest commit point.
7. `basis_release`.
8. `SourceStore`.
9. `ConcurrencyToken`.
10. CAS/current promotion.
11. History con cursor opaco.
12. exact reads.
13. integrity.
14. same-content republish puede crear nueva release.

## Projection Handoff — CLOSED

Congelado:

- `ProjectionTarget = SourceKey + SourceReleaseRef`;
- `project(target)` no relee current;
- exact release provenance;
- CURRENT/OUTDATED por release identity;
- retry mismo target;
- failure no revierte Source;
- `ProjectionStore.get_active/replace_active`.

## Manager generic consumer boundary — CLOSED

Manager usa:

```text
DraftValidationWorkflow
SourceReaderWorkflow
SourcePublicationWorkflow
SourceHistoryWorkflow
ProjectionStatus
ProjectionTarget
ProjectionExecutionResult
```

No usa:

```text
ConfigurationLifecycleWorkflow
ExactSourceReaderWorkflow
ExactSourcePublicationWorkflow
ExactSourceHistoryWorkflow
ExactProjectionWorkflow
expected_source_revision
```

`ManagerModule` no posee campos legacy/exact alternativos.

## OPEN — Manager consumers

Cada uno debe migrarse directamente al contrato genérico:

```text
Navigation        PLANNED / NEXT
Tools             PLANNED
KPI Configuration PLANNED
KPI Definition    PLANNED
```

No crear compatibilidad en Manager para acelerar estos consumers.

## BLOCKED — global qualification

```text
MANAGER-CONSUMER-GLOBAL-QUALIFICATION
BLOCKED
```

hasta cerrar los consumers y ejecutar suites integradas.

## UNVERIFIED

- full Web suite en `59fcd3e...`;
- full ADA suite en `59fcd3e...`;
- consumer integration;
- Docker E2E;
- CI remoto;
- Python 3.14.7 qualification global;
- scan global de legacy fuera de Manager.

## Otros open contracts

Los contratos abiertos de Users runtime, resource topology, retention y otros dominios permanecen en sus documentos especializados. Este cierre no los revalida ni modifica.
