# Configuration Source Storage — Index

Estado: **IN PROGRESS — USERS MANAGER EXACT LIFECYCLE CLOSED**

Checkpoint relevante actual:

```text
SOURCE-1A.1                       Core + Local                 CLOSED / VERIFIED
SOURCE-1A.2                       Blob                         CLOSED / VERIFIED
Projection                        Exact-release Core           CLOSED / VERIFIED
USERS-CANONICAL-PROJECTION-2      Users/Cosmos provider        CLOSED / VERIFIED
MANAGER-ROOT-CANONICAL-CUTOVER    Root Projection transport    CLOSED / VERIFIED
USERS-EXACT-MANAGER-LIFECYCLE     Users Manager consumer       CLOSED / VERIFIED
```

| Archivo | Contenido | Estado |
|---|---|---|
| `01_RELEASE_MODEL.md` | Semántica de releases/versiones. | CURRENT / FROZEN |
| `02_SOURCE_STORE_CONTRACT.md` | Contrato común Local/Blob. | CURRENT / FROZEN |
| `03_CONCURRENCY.md` | Concurrencia y promoción de current. | CURRENT / FROZEN |
| `04_PROJECTION_HANDOFF.md` | Source release exacto hacia Projection y adopción Manager/Users. | CURRENT / FROZEN |
| `05_IMPLEMENTATION_ORDER.md` | Orden/checkpoints de Source/Projection y consumers. | CURRENT PLAN |
| `06_OPEN_CONTRACTS.md` | Contratos cerrados y abiertos. | IN PROGRESS |

Estado actual:

```text
Source Local                       CURRENT
Source Blob                        CURRENT
Projection exact-release           CURRENT
Manager root Projection action     CURRENT
Users Manager exact lifecycle      CURRENT
Users runtime exact provenance     PLANNED
Non-Users legacy Projection align  PLANNED
Other consumer migrations          PLANNED
```

Permanecen pendientes:

- provenance exact-release de `users.runtime`;
- runtime canonical Users cutover;
- consumers administrativos no migrados;
- Projection alignment legacy de Navigation/Tools/KPI/KPI Definitions;
- eliminación legacy sólo después de validar consumers;
- resource topology físico canonical Users Projection;
- orchestration multi-capability cuando exista requisito real;
- retention/cleanup operacional.
