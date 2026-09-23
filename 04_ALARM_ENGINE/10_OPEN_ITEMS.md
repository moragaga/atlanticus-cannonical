# Alarm Engine — Open Items

Estado: **OPEN / EXACT ALARM-TOOL SNAPSHOT CLOSED / OPERATIONAL PROJECTION NEXT**

Checkpoint:

```text
moragaga/atlanticus@880cb692054c2cd78cdc29cf62ab7b16bbd2c3d6
```

## CLOSED / CURRENT

- Pure B.2 resolver.
- Runtime/Delivery resolution contracts.
- READY/BLOCKED atomicity.
- Tool/Evaluator qualification input contracts.
- Command Center `domain/tools`.
- `ToolDependencyManifest`.
- AlarmConfigurationSnapshot v3.
- exact Alarm release -> Tool revision correlation.
- validate/publish Tool drift guard.
- referenced Tool subset capture including inactive Rules and disabled steps.
- historical Tool names/structure evidence.

## 1. Alarm Configuration operational Projection to Cosmos — NEXT

Diseñar e implementar:

```text
ProjectionRecord[AlarmConfigurationSnapshot]
```

en Cosmos.

Debe:
- conservar exact Source release;
- conservar snapshot v3 completo;
- no volver a leer Tool Catalog;
- soportar lectura operacional posterior.

OPEN de diseño:
- database/container;
- partition key;
- item id;
- codec físico;
- connection/settings composition;
- failure/retry semantics.

## 2. B.2 Materialization Process — AFTER

```text
backend/processes/alarms-materialization
```

No implementar en el mismo incremento.

## 3. Tool reconciliation qualification producer

OPEN cómo obtener evidencia GREEN upstream sin reimplementar reconciliation en Alarm backend.

## 4. Evaluator qualification producer

OPEN integración con deployed evaluator registry/catalog.

## 5. Runtime/Delivery/findings stores

OPEN.

## 6. Runtime provenance cleanup

OPEN.

## 7. Runtime Adoption / Effective Head

PLANNED.

## 8. Live Delivery

PLANNED.

## 9. Management Capture

PLANNED.

## 10. Catalog/domain normalization

DEFERRED.

CURRENT:

```text
domain/alarms
-> domain/tools
-> ada-web-tools
```

## 11. Source v2 deployment history

Schema v2 está SUPERSEDED y no existe legacy decoder.

UNVERIFIED si existen releases v2 durables en ambientes objetivo que requieran reset/migración.

## 12. Python baseline

```text
Project 3.14.7
Command Center metadata 3.14.2
```

OPEN / SEPARATE.

## Foco único

Cosmos Projection de Alarm Configuration.
