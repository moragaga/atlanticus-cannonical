# Configuration Source Storage — Index

Estado: **IN PROGRESS — ROOT PROJECTION CUTOVER CLOSED**

Checkpoint:

```text
SOURCE-1A.1                       Core + Local                  CLOSED / VERIFIED
SOURCE-1A.2                       Blob                          CLOSED / VERIFIED
Projection                        Exact-release Core            CLOSED / VERIFIED
USERS-CANONICAL-PROJECTION-2      Users/Cosmos provider         CLOSED / VERIFIED
MANAGER-ROOT-CANONICAL-CUTOVER    Root Projection transport     CLOSED / VERIFIED
Consumer migrations               Domain administrative paths   PLANNED
```

| Archivo | Contenido | Estado |
|---|---|---|
| `01_RELEASE_MODEL.md` | Semántica de releases/versiones. | CURRENT / FROZEN 1A.1 |
| `02_SOURCE_STORE_CONTRACT.md` | Contrato común Local/Blob. | CURRENT / FROZEN 1A.2 |
| `03_CONCURRENCY.md` | Concurrencia y promoción de current. | CURRENT / FROZEN CORE+LOCAL+BLOB |
| `04_PROJECTION_HANDOFF.md` | Source release específico hacia Projection y adopción del root Manager. | CURRENT / FROZEN |
| `05_IMPLEMENTATION_ORDER.md` | Orden y checkpoint de ejecución. | CURRENT PLAN |
| `06_OPEN_CONTRACTS.md` | Contratos cerrados y aún abiertos. | IN PROGRESS / POST-MANAGER-ROOT-CUTOVER |

Estado actual:

```text
Source Local                    CURRENT
Source Blob                     CURRENT
Projection exact-release        CURRENT
Manager root Projection action  CURRENT
Domain admin migrations         PLANNED
Users runtime exact provenance  PLANNED
```

Core Source, Local, Blob, Projection Handoff y el transporte exact-target del root Manager están cerrados.

Permanecen pendientes:
- provenance exact-release de `users.runtime`;
- migración progresiva de consumidores administrativos;
- eliminación de contratos legacy sólo después de validar consumidores;
- otros providers Projection concretos por dominio cuando sean necesarios;
- orchestration multi-capability y derived resolutions cuando exista requisito real;
- retention/cleanup operacional.
