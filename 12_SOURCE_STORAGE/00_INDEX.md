# Configuration Source Storage — Index

Estado: **IN PROGRESS**

Checkpoint:

```text
SOURCE-1A.1  Core + Local  CLOSED / VERIFIED
SOURCE-1A.2  Blob          NEXT
```

| Archivo | Contenido | Estado |
|---|---|---|
| `01_RELEASE_MODEL.md` | Semántica de releases/versiones. | CURRENT / FROZEN 1A.1 |
| `02_SOURCE_STORE_CONTRACT.md` | Contrato común Local/Blob. | CURRENT / FROZEN 1A.1 |
| `03_CONCURRENCY.md` | Concurrencia y promoción de current. | CURRENT / FROZEN CORE+LOCAL |
| `04_PROJECTION_HANDOFF.md` | Source release específico hacia Projection. | PLANNED |
| `05_IMPLEMENTATION_ORDER.md` | Orden y checkpoint de ejecución. | CURRENT PLAN |
| `06_OPEN_CONTRACTS.md` | Contratos aún abiertos. | IN PROGRESS |

Provider actual:

```text
Local  CURRENT
Blob   NEXT
```

Projection y migración de consumidores no forman parte de 1A.1.
