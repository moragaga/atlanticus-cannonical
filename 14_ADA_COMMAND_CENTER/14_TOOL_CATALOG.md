# ADA Command Center — Tool Catalog

Estado: **NEXT / DESIGN FIRST / IMPLEMENTATION PENDING**

## Propósito

Command Center necesita una visión consolidada y read-only de las Tools que puede usar para
resolución de Alarm Configuration.

El siguiente hito debe congelar primero el contrato productor que consumirá B.2.

## Frontera de ownership congelada

ADA Tool Configuration sigue siendo autoridad de:

- `tool_key`;
- display name;
- Tool type/kind;
- Components;
- Subcomponents;
- relaciones;
- topología.

Command Center Tool Catalog es otra capability.

Es un:

```text
consolidator
+
reconciler
+
read-only derived catalog
```

No es:

- Tool authoring;
- una variante visual del editor ADA;
- una segunda source of truth;
- un fork de `ToolConfiguration`/`ToolStructure`;
- una proyección que Command Center deba volver a publicar en Cosmos por defecto.

La diferencia con ADA Tool Configuration debe permanecer explícita en nombres, ownership y
responsabilidad.

## Reality check CURRENT

Al checkpoint:

```text
moragaga/atlanticus@1c67212b21ef2241bcb59173ccb8e9cd237a0219
```

no existe implementación de Command Center Tool Catalog.

Sí existen contratos ADA CURRENT relevantes que deben auditarse/reutilizarse antes de diseñar:

```text
ToolConfiguration
ToolConfigurationKind
ToolStructure
ToolComponent
ToolSubcomponent
Tool projection contracts/stores
```

No inventar otro modelo de Tool si esos contratos ya cubren la topología requerida.

## Consumer responsibility

El catálogo debe permitir posteriormente:

- authoring asistido read-only;
- resolución Tool/Component/Subcomponent;
- validación externa de routing/escalation/visual targets;
- readiness;
- provenance;
- continuidad ante indisponibilidad temporal de fuentes externas.

B.2 consumirá una revisión concreta del catálogo junto con una Alarm Configuration Projection
concreta.

## Identity

`tool_key` permanece identidad de Tool.

Distintas Tools tienen distintas `tool_key`, aunque compartan `display_name`.

No diseñar resolución de colisiones por nombre visible.

El punto CURRENT que genera/enforce unicidad global de `tool_key` debe verificarse antes de fijar el
reconciliador.

## Inputs

La dirección congelada permite múltiples fuentes Tool externas mediante conexiones nombradas.

Cada input debe declararse explícitamente y consumirse read-only desde Command Center.

No asumir:

- Cosmos global;
- discovery arbitrario en cada edición de Rule;
- que Command Center provisiona containers externos;
- que todos los providers Tool son idénticos sin inspección.

## Reconciliation direction

```text
named external Tool projection inputs
        ↓
validate/normalize against CURRENT contracts
        ↓
reconcile
        ↓
Command Center Tool Catalog snapshot
        ↓
durable revision/current/LKG
        ↓
B.2
```

Este diagrama es dirección, no un schema físico ya implementado.

## Durable state

Blob continúa como target durable de dirección para el estado/revisión consolidado.

El contrato físico exacto sigue OPEN:

- container;
- namespace/path;
- manifest/current pointer;
- revision identity;
- history retention;
- codec/document shape.

No fijar estos campos antes de cerrar el contrato logical del catálogo.

## Availability semantics congeladas como conceptos

Debe distinguirse al menos:

### AVAILABLE

La Tool fue observada correctamente en una reconciliación válida.

### STALE

La fuente externa no pudo observarse actualmente y se conserva la última Tool válida conocida/LKG.

Indisponibilidad temporal no equivale a eliminación.

### MISSING

Una fuente reconciliada correctamente confirma que una Tool previamente conocida ya no está
disponible según el contrato de esa fuente.

La forma exacta de representar/retener MISSING todavía no está congelada.

### UNRESOLVED Alarm reference

No es un estado del Tool Catalog equivalente a los anteriores.

Es un resultado de B.2 cuando una Alarm Configuration referencia una `tool_key` que no puede
resolverse contra la revisión de catálogo usada.

```text
Tool availability state
!=
Alarm reference resolution finding
```

## Re-resolution

La misma Alarm Source revision puede producir una nueva resolución cuando cambia el Tool Catalog.

```text
Alarm A17 + Catalog T40 → unresolved
Alarm A17 + Catalog T41 → resolved
```

No es necesario republicar A17 si su contenido no cambió.

## Startup / failure direction

Command Center debe poder usar el último catálogo durable válido cuando una fuente externa esté
temporalmente indisponible, sin presentar STALE como confirmación current.

La ausencia total de catálogo puede limitar authoring asistido/resolution, pero no debe inventar
Tools ni mutar Alarm Configuration.

## Siguiente hito exacto

```text
COMMAND-CENTER-TOOL-CATALOG-CONTRACT
```

Scope del próximo chat:

1. auditar contratos Tool CURRENT productores;
2. definir el aggregate/snapshot mínimo del consolidator;
3. definir entry identity + provenance + availability semantics;
4. definir consumer interface que B.2 necesitará;
5. testear invariantes del contrato puro.

Fuera de ese primer incremento:

- Cosmos reconciliation runtime;
- Blob persistence física;
- cadence/retry;
- UI final;
- B.2 implementation;
- Runtime/Delivery changes.

## Non-goals

No crear:

- legacy adapters;
- aliases de compatibilidad;
- duplicación de Tool authoring;
- segunda Tool Projection Cosmos de Command Center por simetría;
- schemas especulativos no derivados de código/contratos CURRENT.
