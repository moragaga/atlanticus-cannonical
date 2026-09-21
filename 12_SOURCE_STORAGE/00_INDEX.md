# Configuration Source Storage — Index

Estado: **CURRENT**

Checkpoint relevante:

```text
SOURCE-1A.1                         Core + Local                  CLOSED / VERIFIED
SOURCE-1A.2                         Blob                          CLOSED / VERIFIED
Projection                          Exact-release Core            CLOSED / VERIFIED
MANAGER-GENERIC-SOURCE-PROJECTION   Generic Manager handoff       CLOSED / VERIFIED
ADA-STORAGE-NAMESPACE               Logical namespace             CLOSED / VERIFIED
TOOL-PROJECTION-PERSISTENCE         Local + Cosmos                CLOSED / VERIFIED
TOOL-PERSISTENCE-COMPOSITION        Provider composition          CLOSED / VERIFIED
```

| Archivo | Contenido | Estado |
|---|---|---|
| `01_RELEASE_MODEL.md` | Semántica de releases/versiones. | CURRENT / FROZEN |
| `02_SOURCE_STORE_CONTRACT.md` | Contrato común Local/Blob. | CURRENT / FROZEN |
| `03_CONCURRENCY.md` | Concurrencia y promoción de current. | CURRENT / FROZEN |
| `04_PROJECTION_HANDOFF.md` | Source exacta hacia Projection y durable Tool read. | CURRENT / FROZEN |
| `05_IMPLEMENTATION_ORDER.md` | Orden/checkpoints históricos. | HISTORICAL PLAN |
| `06_OPEN_CONTRACTS.md` | Contratos abiertos no cerrados aquí. | CURRENT OPEN ITEMS |

## Provider axes CURRENT

```text
Source     Local | Blob
Projection Local | Cosmos
```

No asumir acoplamiento obligatorio:

```text
Local Source -> Local Projection only
Blob Source  -> Cosmos Projection only
```

Las combinaciones son independientes cuando el consumer lo soporta.

## Namespace

```text
physical container
application namespace
tool namespace
SourceKey
```

son identidades distintas.

Source Tool root:

```text
<application>/<tool>
```

`SourceStore` agrega `sources/<SourceKey>`.

Tool Projection local:

```text
<base>/<application>/<tool>/projections
```

Tool Projection Cosmos:

```text
partition_key = <application>/<tool>
```

## Current gap

La infraestructura/provider composition está cerrada.

Lo abierto es el consumer ADA Generic real:

```text
ADA-GENERIC-OPERATIONAL-BOOTSTRAP
PLANNED / NEXT
```

No reabrir Projection Core para resolverlo.
