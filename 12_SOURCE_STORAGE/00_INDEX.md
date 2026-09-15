# Configuration Source Storage — Index

Estado: **IN PROGRESS — MANAGER GENERIC HANDOFF CLOSED**

Checkpoint relevante actual:

```text
SOURCE-1A.1                         Core + Local                  CLOSED / VERIFIED
SOURCE-1A.2                         Blob                          CLOSED / VERIFIED
Projection                          Exact-release Core            CLOSED / VERIFIED
USERS-CANONICAL-PROJECTION-2        Users/Cosmos provider         CLOSED / VERIFIED
MANAGER-GENERIC-SOURCE-PROJECTION   Generic Manager handoff       CLOSED / VERIFIED
```

| Archivo | Contenido | Estado |
|---|---|---|
| `01_RELEASE_MODEL.md` | Semántica de releases/versiones. | CURRENT / FROZEN |
| `02_SOURCE_STORE_CONTRACT.md` | Contrato común Local/Blob. | CURRENT / FROZEN |
| `03_CONCURRENCY.md` | Concurrencia y promoción de current. | CURRENT / FROZEN |
| `04_PROJECTION_HANDOFF.md` | Source release exacta hacia Projection y Manager genérico. | CURRENT / FROZEN |
| `05_IMPLEMENTATION_ORDER.md` | Orden/checkpoints de Source/Projection y consumers. | CURRENT PLAN |
| `06_OPEN_CONTRACTS.md` | Contratos cerrados y abiertos. | IN PROGRESS |

Estado actual:

```text
Source Local                         CURRENT
Source Blob                          CURRENT
Projection exact-release             CURRENT
Manager generic Source/Projection    CURRENT

Navigation Manager consumer          PLANNED / NEXT
Tools Manager consumer               PLANNED
KPI Configuration Manager consumer   PLANNED
KPI Definition Manager consumer      PLANNED
Global consumer qualification         BLOCKED
```

Permanecen pendientes otros frentes de Source/Projection ya documentados fuera de este cierre.

No reintroducir rutas exact/legacy dentro de Manager.
