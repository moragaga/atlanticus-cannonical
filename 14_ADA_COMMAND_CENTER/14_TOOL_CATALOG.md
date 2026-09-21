# ADA Command Center — Tool Catalog

Estado: **FROZEN DIRECTION / IMPLEMENTATION PENDING**

## Propósito

Command Center necesita una visión consolidada y read-only de las Tools que puede usar para resolución de Alarm Configuration.

El Tool Catalog permite:

- authoring asistido;
- resolución Tool/Component/Subcomponent;
- validación de routing/escalation/visual targets;
- readiness;
- provenance;
- continuidad ante indisponibilidad temporal de fuentes externas.

## Ownership

Tool Configuration sigue siendo autoridad de:

- `tool_key`;
- display name;
- Tool type/tier;
- Components;
- Subcomponents;
- relaciones;
- topología.

Command Center Tool Catalog es estado derivado/reconciliado.

No es:

- Tool authoring;
- una segunda source of truth;
- una proyección que Command Center deba volver a publicar en Cosmos por defecto.

## Identity

`tool_key` es la identidad de Tool.

Distintas Tools tienen distintas `tool_key`, aunque compartan el mismo `display_name`.

El catálogo se indexa conceptualmente por `tool_key`.

No se diseña resolución de colisiones por nombre visible.

## Inputs

El catálogo puede consumir múltiples Cosmos externos.

Cada input se declara explícitamente mediante una conexión nombrada y el contrato físico necesario para leer la Tool projection correspondiente.

No asumir un Cosmos global.

Las conexiones externas son consumo read-only desde la perspectiva de Command Center.

Command Center no provisiona, crea ni modifica containers externos sólo por consumirlos.

## Reconciliation

Flujo objetivo:

```text
named external Cosmos inputs
        ↓
read confirmed Tool projections
        ↓
validate/normalize
        ↓
reconcile
        ↓
CommandCenterToolCatalog snapshot
        ↓
Blob durable revision + current/LKG
```

El reconciliador no hace Tool discovery arbitrario durante cada edición de Alarm Rule.

## Durable state

El estado/revisión consolidado se conserva en Blob.

No se agrega un Cosmos de Command Center únicamente para duplicar Tool topology.

El contrato físico exacto de:

- container;
- path;
- manifest/current pointer;
- revision identity;
- history retention;

queda abierto para el incremento de implementación.

## Availability semantics

Debe distinguirse al menos:

### AVAILABLE

La Tool fue observada correctamente en una reconciliación válida.

### STALE

La fuente externa no pudo observarse actualmente y se conserva la última Tool válida conocida/LKG.

Indisponibilidad de una conexión no equivale a eliminación de la Tool.

### MISSING

Una fuente que pudo reconciliarse correctamente confirma que una Tool previamente conocida ya no está disponible según el contrato de esa fuente.

La política exacta de retención/representación del entry MISSING queda abierta, pero no debe confundirse con STALE.

### UNRESOLVED reference

Una Alarm Configuration puede referenciar una `tool_key` que todavía no existe en el catálogo actual.

Esto es un resultado de resolución, no un estado inválido de la Alarm Source revision.

## Consumer contract

Alarm Configuration UI consume el catálogo como read-only.

B.2 consume una revisión concreta del catálogo para materializar readiness/provenance.

La misma Alarm Source revision puede producir una nueva resolución al cambiar la Tool Catalog revision.

```text
Alarm A17 + Catalog T40 → unresolved
Alarm A17 + Catalog T41 → resolved
```

No es necesario republicar A17 si su contenido no cambió.

## Startup / failure

Command Center debe poder levantar usando el último catálogo durable válido aunque una o más conexiones externas estén temporalmente indisponibles.

La Web debe hacer visible freshness/readiness y no presentar una observación stale como confirmación current.

La ausencia total de catálogo puede limitar authoring asistido/resolution, pero no debe inventar Tools ni mutar Alarm Configuration.

## Non-goals

Este contrato no define todavía:

- cadence exacta;
- trigger manual/automático;
- formato físico del snapshot Blob;
- lifecycle de clientes Cosmos;
- UI detallada del catálogo;
- retry/backoff;
- métricas operacionales.
