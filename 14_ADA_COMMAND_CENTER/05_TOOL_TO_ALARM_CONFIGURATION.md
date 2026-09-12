# ADA Command Center — Tool to Alarm Configuration

Estado: **FROZEN SEMANTICS / INTEGRATION PENDING**

Tool Configuration es dueña de:

- `tool_key`;
- Tool type/tier;
- Components;
- Subcomponents;
- relaciones;
- topología.

Alarm Configuration guarda referencias.

## Cadena

```text
Tool Configuration
        ↓
Tool Projection / confirmed topology
        ↓
Command Center Tool Catalog
        ↓
Alarm Configuration
        ↓
B.2 Configuration Resolution
        ↓
Resolved Alarm Configuration
        ↓
Alarm Runtime
```

## Catálogo confirmado

Command Center debe leer una topología confirmada.

No descubrir Tools arbitrariamente durante cada edición de Rule.

La implementación histórica SharePoint/Cosmos puede cambiar con Blob, pero se conserva la invariante:

> sólo topología reconciliada/confirmada puede alimentar Alarm Configuration.

## B.2 valida

Entre otros:

- Tool existe;
- Component existe;
- Subcomponent existe;
- Subcomponent pertenece al Component esperado;
- Tool type permite projection mode;
- routing/escalation apunta a Tools válidas;
- Message existe/activo y pertenece a GLOBAL o misma family;
- evaluator existe;
- Special Condition existe y respeta family/priority group.

## Tool types

### PROCESS
`process_projection_mode`:
- GENERIC;
- DISTRIBUTED.

### INTEGRATED_OPERATIONS
`process_projection_mode = None`.

La geometría visual queda fuera de Alarm Core.

## Principio

Command Center configura alarmas sobre topología publicada por las Tools.

No duplica esa topología como segunda source of truth.
