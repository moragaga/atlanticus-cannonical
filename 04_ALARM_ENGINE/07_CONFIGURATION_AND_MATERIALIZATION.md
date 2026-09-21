# Alarm Engine — Configuration and Materialization

Estado: **CURRENT SEMANTICS / ALARM SOURCE + BASE PROJECTION + TOOL CATALOG PRODUCER CURRENT / B.2 OPEN**

## Invariante central

`LATEST SAVED = LATEST VALID`

`VALID` significa aquí **intrínsecamente válida como Alarm Configuration persistible**.

No significa que todas las dependencias externas estén disponibles o resolubles en ese instante.

Una working copy intrínsecamente inválida/incompleta no se convierte en revisión persistida
autoritativa.

## Implementación CURRENT previa a B.2

Bajo:

```text
scopes/ada-command-center/web/alarms/configuration
```

ya existen:

```text
AlarmConfiguration aggregate
→ full-revision intrinsic validation
→ Source/Release
→ exact base Projection[AlarmConfiguration]
→ Manager workflow/history
```

La base Projection no resuelve dependencias externas y tiene:

```text
ProjectionTarget.dependencies == ()
```

No confundirla con `ResolvedAlarmConfiguration`.

También existe el productor de topología Tool consolidada:

```text
scopes/ada-command-center/backend/tools/catalog
```

```text
named Tool Projection inputs
→ ToolCatalogConsolidator
→ ToolCatalogSnapshot
→ Blob ToolCatalogStore CURRENT
```

Y existe un read model de authoring en Alarm Configuration:

```text
ToolCatalogStore
→ AlarmToolReferenceReader
→ AlarmToolReferenceCatalog
```

Ese read model no es B.2 y no altera la validez intrínseca del aggregate.

## Fases

Estado actual:

- working copy — **CURRENT mediante Manager workspace**;
- intrinsic pre-save validation — **CURRENT**;
- persisted valid Alarm Source revision — **CURRENT contract**;
- base Alarm Configuration Projection — **CURRENT**;
- Command Center Tool Catalog producer — **CURRENT V1**;
- Tool reference authoring read model — **CURRENT V1**;
- structured authoring UI — **PLANNED / NEXT**;
- external resolution/materialization — **PLANNED / B.2**;
- capability readiness — **PLANNED / B.2**;
- Runtime adoption — **EXISTING RUNTIME / RECONCILIATION OPEN**;
- EFFECTIVE — **RECONCILIATION OPEN**.

## Intrinsic pre-save validation CURRENT

Valida el candidate completo en aquello que pertenece a Alarm Configuration, incluyendo:

- identity/uniqueness;
- `rule_name` uniqueness within family;
- priority invariants;
- Special Condition references dentro del aggregate;
- Message references dentro del aggregate;
- deactivation/reappearance structure;
- escalation structure;
- parameter keys;
- parameter values limitados a `str | float | bool`.

CURRENT implementation también fija:

- `message_key` único dentro del aggregate;
- una referencia a Message inactivo sigue siendo intrínsecamente válida.

Un finding intrínseco blocking rechaza save/publication del aggregate.

La inexistencia actual de Tool/evaluator no pertenece a esta validación intrínseca.

## Tool Catalog CURRENT antes de B.2

`ToolCatalogSnapshot` contiene entradas ordenadas por `tool_key` con:

```text
tool_key
display_name
kind
source_release_id
ToolStructure
```

Su `revision` se deriva de forma determinística del contenido semántico del catálogo.
`generated_at_utc` no participa en esa revisión.

El consolidator exige que todos los inputs configurados entreguen una Projection activa válida con
`ToolConfiguration.structure` antes de reemplazar CURRENT.

Si un input falla, falta, tiene payload inválido, no tiene structure o repite `tool_key`:

```text
refresh fails
→ no partial snapshot
→ CURRENT previous blob remains untouched
```

V1 no implementa AVAILABLE/STALE/MISSING ni LKG separado. El último CURRENT publicado correctamente
es el estado durable disponible.

## External resolution PLANNED

B.2 podrá combinar una Alarm Configuration Projection concreta con una Tool Catalog revision
concreta para evaluar, entre otros:

- evaluator disponible;
- Tool disponible;
- Component/Subcomponent resoluble;
- Tool type/projection mode;
- visual targets;
- routing/escalation targets;
- external topology drift.

Un finding externo no convierte retrospectivamente la Alarm Source revision en inválida.

Puede impedir que una capability quede READY.

```text
VALID
!=
FULLY RESOLVED
!=
READY FOR EVERY CAPABILITY
```

## Preconfiguration

Se permite persistir una Rule que referencia una Tool/evaluator todavía no disponible, siempre que
el contrato intrínseco sea válido.

Esa Rule:

- no se considera removed;
- no se considera disabled;
- conserva su historia;
- puede re-resolverse posteriormente sin nueva Alarm Source revision.

El read model de authoring no cambia esta semántica: catálogo ausente o key no sugerida no transforma
la configuración en inválida.

## Re-resolution

La identidad/provenance de resolución futura debe distinguir al menos la Alarm Source revision y la
Tool Catalog revision utilizada.

```text
Alarm Source A17 + Tool Catalog T40
→ unresolved Tool

Alarm Source A17 + Tool Catalog T41
→ resolved Tool
```

A17 no cambia.

## Runtime / Delivery

Runtime y Delivery derivarán de la misma resolución validada/provenance.

La readiness puede diferir por capability.

Una dependencia exclusivamente visual/routing puede dejar Delivery no READY sin impedir una
evaluación Runtime que no necesita esa dependencia.

Delivery no despacha hacia referencias externas no resueltas.

Delivery no puede liderar la configuración EFFECTIVE de Runtime.

No existe todavía implementación B.2 ni `ResolvedAlarmConfiguration` en `main` al checkpoint de este
cierre.

## Runtime CURRENT a reconciliar

El runtime existente conserva revision strings históricos:

```text
alarm_configuration_revision
tool_registry_revision
```

La reconciliación futura debe reemplazar limpiamente ese provenance donde corresponda. No introducir
adapters temporales ni doble contrato.

## Parameters

Alarm Configuration no incorpora schemas particulares por evaluator.

El contrato genérico permanece:

```text
mapping[str, str | float | bool]
```

Los nombres y semántica de parameters pertenecen al evaluator/desarrollador.

## Storage

Alarm Configuration adopta Source/Release CURRENT de Atlanticus y Blob como provider durable objetivo
en dominios migrados.

El package Alarm Configuration recibe `SourceStore` explícitamente; el binding productivo Blob de la
aplicación Command Center permanece OPEN.

Tool Catalog V1 ya implementa un `BlobToolCatalogStore` sobre `StorageClient` con `container_name` y
`blob_name` explícitos. No introduce Cosmos propio de Command Center.

La composición productiva que conecte los inputs Tool reales, Storage credentials y la ejecución del
refresh permanece OPEN.
