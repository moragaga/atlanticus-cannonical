# Atlanticus Canonical Context — Index

Estado: **CURRENT — DYNAMIC PROCESS DEPLOYMENT RESOURCES CLOSED; EXTENSION INTEGRATION QUALIFICATION NEXT**

## Autoridad de este cierre

```text
Implementation        moragaga/atlanticus@5c40faed4df7f3d7b6db79251144a9ec09e09e91
Canonical pre-replace moragaga/atlanticus-cannonical@15a51f70396726a2ad3b88d1afc66ce8cfff3300
Decisions             moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Git                   SOLO LECTURA
```

## Estado por frente

| Ubicación | Estado relevante |
|---|---|
| `01_CURRENT_STATE.md` | Dynamic deployment resource boundary CURRENT / locally qualified. |
| `02_ARCHITECTURE.md` | `deployment.resources.json` is the consumer-owned effective resource source for distributed processes. |
| `03_DECISIONS_CURRENT.md` | `pyproject.toml` and base Compose are no longer resource authority for distributions. |
| `07_VALIDATION_BASELINE.md` | Process deployment gate GREEN: 111 tests plus Ruff/format/launcher checks. |
| `16_KPI_BACKEND_RECOVERY/` | KPI state from previous closure remains unchanged. |
| `17_DISTRIBUTION_AND_TOOLING/` | Resource boundary closed; focused extension-integration qualification is next. |

## Checkpoints

```text
OPERATIONAL-DATA-INPUT-CONTRACT                     CLOSED / CURRENT
KPI-RUNTIME-DATA-INPUT-MIGRATION                    CLOSED / CURRENT
DYNAMIC-DISTRIBUTED-DEPLOYMENT-RESOURCES            CLOSED / CURRENT
LOCAL-WORKSPACE-RUNTIME-INPUT-ALIGNMENT              CLOSED / CURRENT
PROCESS-DEPLOYMENT-GATE-REALIGNMENT                  CLOSED / CURRENT
EXTENSION-RESOURCE-INTEGRATION-QUALIFICATION         PLANNED / NEXT
FULL-ARTIFACT-AND-ENV-DETAIL-QUALIFICATION           PLANNED / SEPARATE NEXT STAGE
```

## Siguiente frontera única

```text
ATLANTICUS-EXTENSION-RESOURCE-INTEGRATION-QUALIFICATION
```

Objetivo:

- calificar `integrate` cuando una distribución CURRENT recibe un componente nuevo;
- demostrar que sizing existente se conserva;
- demostrar que el componente nuevo recibe `0.5 vCPU / 1.0 GiB`;
- demostrar rollback de `deployment.resources.json` si falla la publicación;
- no reabrir el diseño de recursos dinámicos;
- no mezclar `.env.detail`, artifact-wide qualification, Alarm ni Web.
