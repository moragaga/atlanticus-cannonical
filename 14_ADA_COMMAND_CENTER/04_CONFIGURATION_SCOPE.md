# ADA Command Center — Configuration Scope

Estado: **CURRENT / CORE FLOW + TOOL REFERENCE BACKEND IMPLEMENTED / STRUCTURED UI NEXT**

Command Center es owner de Alarm Configuration.

No replica el Manager ADA como implementación paralela. Alarm Configuration CURRENT reutiliza la
capability genérica `atlanticus.web.manager`; el layout/editor específico permanece bajo ownership
de Command Center.

## Implementación CURRENT

Paquete:

```text
scopes/ada-command-center/web/alarms/configuration
```

Cadena durable implementada:

```text
Manager Workspace
→ intrinsic validation
→ Source publication/history
→ Alarm Configuration Projection
```

El Manager module recibe explícitamente stores, `SourceKey`, principal, `access_key` y metadatos de
composición. No fija el provider productivo ni el shell final de Command Center.

## NO Tool authoring

Command Center no crea/edita:

- Tool identity;
- Tool type/kind;
- Components;
- Subcomponents;
- Tool topology.

Eso pertenece a ADA Tool Configuration.

Command Center consume una vista consolidada/read-only mediante `ToolCatalogStore`.

## Alarm Configuration aggregate

La unidad editable/publicable CURRENT contiene atómicamente:

- Alarm Rules;
- Message Catalog.

Tool Catalog no forma parte de este payload editable y tiene lifecycle/revisión independiente.

La incorporación del catálogo no cambió el documento durable de `AlarmConfiguration`.

## Tool reference authoring backend CURRENT

Alarm Configuration dispone de:

```text
AlarmToolReferenceReader
→ AlarmToolReferenceCatalog
```

El reader traduce el Tool Catalog CURRENT a opciones backend de:

```text
Tool
→ Component
→ visible Subcomponent address
```

Cada subcomponent reference conserva:

```text
owner_component_key
subcomponent_key
display_name
```

La topología no se recalcula en Alarm Configuration: se reutilizan las operaciones CURRENT de
`ToolStructure`.

`STRATEGIC` se omite de las sugerencias porque `ToolStructure` no define proyección de alarmas para
ese kind.

Esto es **authoring assistance**, no resolución B.2.

## Authoring no restrictivo

Se conserva congelado:

```text
catalog available
!=
requirement to create/save Alarm Configuration
```

Si no existe catálogo CURRENT, `AlarmToolReferenceReader.load()` devuelve `None`.

Si una key no está presente en las opciones, Alarm Configuration sigue validándose únicamente por su
contrato intrínseco CURRENT.

No se agregó enforcement externo a:

- `AlarmConfiguration.from_document()`;
- draft validation;
- Source publication;
- base Projection.

## Tool identity

Alarm Configuration persiste referencias escalares:

- `tool_key`;
- `component_key` donde corresponda;
- `owner_component_key` + `subcomponent_key` donde corresponda.

`display_name` es presentación y no identidad.

## Evaluator / parameters

`evaluator_key` identifica código de evaluator registrado por desarrollo.

Alarm Configuration no define schemas particulares por evaluator.

`parameters` conserva:

```text
mapping[str, str | float | bool]
```

La existencia del evaluator y el uso correcto de sus parámetros pertenecen a
resolution/readiness/ejecución.

## Projection base CURRENT

```text
Alarm SourceRelease exacta
→ AlarmConfigurationProjectionBuilder
→ AlarmConfiguration Projection
```

No tiene dependencias externas:

```text
ProjectionTarget.dependencies == ()
```

Tool Catalog y evaluator resolution entran después, en B.2.

## Persistencia

Alarm Configuration adopta Source/Release CURRENT de Atlanticus.

Para dominios migrados, Blob continúa siendo el provider durable objetivo.

El package CURRENT recibe `SourceStore`; el binding físico productivo de Command Center permanece
abierto para la composición de aplicación.

## Web editor CURRENT

La UI actual sigue siendo Document mode.

No se modificaron `layout.py` ni callbacks durante el hito Tool References.

Permanece OPEN y es el siguiente foco único:

```text
ALARM-CONFIGURATION-STRUCTURED-AUTHORING-V1
```

Debe consumir el read model ya implementado para presentar Tool/Component/Subcomponent sin crear un
schema durable paralelo y manteniendo entrada manual/fallback compatible con authoring no
restrictivo.

Message UI, editor visual completo de Rules y editor de parameters pueden continuar incrementalmente,
pero no deben mezclarse automáticamente en el mismo incremento si amplían el foco.
