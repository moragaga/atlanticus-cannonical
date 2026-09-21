# ADA Command Center — Tool to Alarm Configuration

Estado: **FROZEN SEMANTICS / ALARM SIDE CURRENT / TOOL CATALOG NEXT**

ADA Tool Configuration es dueña de:

- `tool_key`;
- Tool type/tier;
- Components;
- Subcomponents;
- relaciones;
- topología.

Alarm Configuration guarda referencias.

`tool_key` es identidad estable de la Tool. Dos Tools distintas no comparten `tool_key`;
`display_name` puede repetirse y no participa de la identidad.

## Frontera de variante

El siguiente componente de Command Center **no es el Tool Configuration authoring de ADA**.

Es una capability distinta:

```text
ADA Tool Configuration(s)
        ↓ durable Tool projections
Command Center Tool Catalog
        = consolidator / reconciler / read-only consumer
```

Debe reutilizar o adaptar explícitamente los contratos CURRENT de Tool Configuration/ToolStructure
cuando corresponda. No debe inventar un fork de Tool authoring ni convertirse en segunda source of
truth.

## Cadena CURRENT / PLANNED

```text
ADA Tool Configuration(s)                    CURRENT fuera de Command Center
        ↓
external Tool projections                    CURRENT por Tool/provider donde aplique
        ↓
Command Center Tool Catalog                  NEXT / NOT YET IMPLEMENTED
        ↓
                 + Alarm Configuration Projection   CURRENT
                 ↓
B.2 Configuration Resolution                 PLANNED
        ↓
Resolved Alarm Configuration                 PLANNED
        ↓
Runtime / Delivery capability readiness      PLANNED
```

Alarm Configuration puede usar el Tool Catalog para authoring asistido, pero el catálogo no forma
parte de su Source aggregate.

## Tool Catalog

Command Center no descubre Tools arbitrariamente durante cada edición de Rule ni durante cada
resolución.

La dirección congelada es un Tool Catalog reconciliado/read-only que mantiene una visión
durable/LKG de las topologías externas conocidas.

El catálogo:

- no crea ni edita Tools;
- no es una segunda source of truth;
- no se replica en un Cosmos propio sólo para duplicar Tool topology;
- tiene lifecycle/revisión independiente de Alarm Configuration;
- puede consolidar múltiples conexiones Cosmos nombradas;
- conserva provenance suficiente para saber de qué observación externa provino una Tool.

El contrato exacto del snapshot/entry/provenance y el binding Blob permanecen abiertos para el
siguiente hito.

## Authoring vs resolution

Sólo topología reconciliada/confirmada puede usarse para RESOLVER una referencia Tool.

Esto no significa que la Tool deba existir para PERSISTIR una Alarm Configuration intrínsecamente
válida.

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

B.2 no está implementado en `atlanticus:main` al checkpoint de este cierre.

### Intrinsic validation

La mayor parte de esta frontera ya quedó implementada en Alarm Configuration:

- alarm identity/uniqueness;
- priority invariants;
- Message references dentro del mismo aggregate;
- deactivation/reappearance structure;
- escalation structure;
- Special Condition references dentro del aggregate;
- parameter key/value shape.

### External resolution PLANNED

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

La misma Alarm Source revision puede resolverse nuevamente contra una revisión posterior del Tool
Catalog.

```text
Alarm Source A17 + Tool Catalog T40
→ Tool unresolved

Alarm Source A17 + Tool Catalog T41
→ Tool resolved
```

La aparición de una Tool no obliga a republicar A17 si Alarm Configuration no cambió.

## Tool types CURRENT de ADA

El contrato CURRENT de ADA Tool Configuration incluye:

```text
PROCESS
INTEGRATED_OPERATIONS
STRATEGIC
```

Para Alarm Configuration permanece congelado:

### PROCESS

`process_projection_mode`:

- GENERIC;
- DISTRIBUTED.

### INTEGRATED_OPERATIONS

`process_projection_mode = None`.

### STRATEGIC

No inventar comportamiento visual específico sin contrato/evidencia adicional.

La geometría visual queda fuera de Alarm Core.

## Principio

Command Center configura referencias hacia Tools y las resolverá contra una topología consolidada y
reconciliada.

No duplica Tool authoring ni convierte disponibilidad externa temporal en invalidez intrínseca de
Alarm Configuration.
