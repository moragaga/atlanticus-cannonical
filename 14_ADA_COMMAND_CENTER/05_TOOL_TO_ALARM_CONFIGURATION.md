# ADA Command Center — Tool to Alarm Configuration

Estado: **FROZEN SEMANTICS / REFINED / INTEGRATION PENDING**

Tool Configuration es dueña de:

- `tool_key`;
- Tool type/tier;
- Components;
- Subcomponents;
- relaciones;
- topología.

Alarm Configuration guarda referencias.

`tool_key` es identidad estable de la Tool. Dos Tools distintas no comparten `tool_key`; `display_name` puede repetirse y no participa de la identidad.

## Cadena

```text
Tool Configuration
        ↓
external Tool projections / confirmed topology
        ↓
Command Center Tool Catalog
        ↓
Alarm Configuration
        ↓
B.2 Configuration Resolution
        ↓
Resolved Alarm Configuration
        ↓
Runtime / Delivery capability readiness
```

## Tool Catalog

Command Center no descubre Tools arbitrariamente durante cada edición de Rule.

Consume un Tool Catalog reconciliado/read-only que mantiene una visión durable/LKG de las topologías externas conocidas.

El catálogo:

- no crea ni edita Tools;
- no es una segunda source of truth;
- no se replica en un Cosmos propio sólo para duplicar Tool topology;
- se persiste como estado/revisión durable en Blob;
- puede consolidar múltiples conexiones Cosmos nombradas;
- conserva provenance suficiente para saber de qué observación externa provino una Tool.

La definición física del Blob y la cadencia/trigger de reconciliación quedan abiertas para implementación.

## Authoring vs resolution

Sólo topología reconciliada/confirmada puede usarse para RESOLVER una referencia Tool.

Esto no significa que la Tool deba existir para PERSISTIR una Alarm Configuration intrínsecamente válida.

Se permite preconfigurar referencias aún no resolubles.

Por tanto:

```text
VALID CONFIGURATION
!=
FULLY RESOLVED CONFIGURATION
```

Una referencia no resuelta genera findings/readiness específicos. No equivale a:

- Rule inválida;
- Rule disabled;
- Rule removed.

## B.2 valida y resuelve

### Intrinsic validation

Entre otros:

- alarm identity/uniqueness;
- priority invariants;
- Message references dentro del mismo Alarm Configuration aggregate;
- deactivation/reappearance structure;
- escalation structure;
- Special Condition structure;
- parameter key/value shape.

### External resolution

Entre otros:

- evaluator disponible;
- Tool disponible;
- Component disponible;
- Subcomponent disponible;
- Subcomponent pertenece al Component esperado;
- Tool type permite projection mode;
- routing/escalation apunta a referencias resolubles;
- visual targets son resolubles contra topología confirmada.

Un finding externo puede impedir una capability concreta sin invalidar la Alarm Source revision.

## Re-resolution

La misma Alarm Source revision puede resolverse nuevamente contra una revisión posterior del Tool Catalog.

```text
Alarm Source A17 + Tool Catalog T40
→ Tool unresolved

Alarm Source A17 + Tool Catalog T41
→ Tool resolved
```

La aparición de una Tool no obliga a republicar A17 si Alarm Configuration no cambió.

## Tool types

### PROCESS

`process_projection_mode`:

- GENERIC;
- DISTRIBUTED.

### INTEGRATED_OPERATIONS

`process_projection_mode = None`.

La geometría visual queda fuera de Alarm Core.

## Principio

Command Center configura referencias hacia Tools y las resuelve contra una topología reconciliada.

No duplica Tool authoring ni convierte disponibilidad externa temporal en invalidez intrínseca de Alarm Configuration.
