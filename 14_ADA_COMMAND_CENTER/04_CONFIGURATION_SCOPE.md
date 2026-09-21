# ADA Command Center — Configuration Scope

Estado: **CURRENT / ALARM CONFIGURATION CORE FLOW IMPLEMENTED / SPECIALIZED UI OPEN**

Command Center es owner de Alarm Configuration.

No replica el Manager ADA como implementación paralela. Alarm Configuration CURRENT reutiliza la
capability genérica `atlanticus.web.manager`; el layout/editor específico permanece bajo ownership
de Command Center.

## Implementación CURRENT

Paquete:

```text
scopes/ada-command-center/web/alarms/configuration
```

Cadena implementada:

```text
Manager Workspace
→ intrinsic validation
→ Source publication/history
→ Alarm Configuration Projection
```

El Manager module es reusable y recibe explícitamente:

- `SourceStore`;
- `ProjectionStore[AlarmConfiguration]`;
- `SourceKey`;
- principal provider;
- `access_key`;
- nombres/ruta/metadatos de composición.

No fija el provider productivo ni el shell final de Command Center.

## NO Tool authoring

Command Center no crea/edita:

- Tool identity;
- Tool type;
- Components;
- Subcomponents;
- Tool topology.

Eso pertenece a ADA Tool Configuration.

Command Center consumirá una vista reconciliada/read-only mediante su Tool Catalog.

## Alarm Configuration aggregate

La unidad editable/publicable CURRENT contiene atómicamente:

- Alarm Rules;
- Message Catalog.

El Tool Catalog no forma parte de este payload editable y tendrá lifecycle/revisión independiente.

### Alarm Rules

Administra el contrato ya implementado:

- identity;
- names/title/cause;
- active/visibility;
- Special Condition;
- kind/criticality/category/areas/color;
- evaluator;
- parameters;
- priority group/order;
- messages;
- reappearance;
- default deactivation;
- escalation;
- visual targets.

### Message Catalog

- GLOBAL;
- FAMILY;
- stable `message_key`;
- display_text;
- active/inactive;
- optional deactivation override.

CURRENT implementation exige `message_key` único dentro del aggregate completo porque las Rules
referencian mensajes mediante la key escalar.

Una referencia a un Message inactivo permanece intrínsecamente válida. La disponibilidad de ese
contenido para Delivery/readiness no se resuelve en Alarm Configuration.

### Evaluator / parameters

`evaluator_key` identifica código de evaluator registrado por desarrollo.

Alarm Configuration no define schemas particulares por evaluator.

`parameters` conserva el contrato genérico:

```text
mapping[str, str | float | bool]
```

La validación intrínseca exige keys válidas y valores de esos tres tipos.

Los nombres de parámetros, su significado y su uso pertenecen al evaluator/desarrollador.

La existencia del evaluator y el uso correcto de sus parámetros pertenecen a
resolution/readiness/ejecución.

Una configuración puede persistirse antes de que el evaluator o una Tool referenciada estén
disponibles. Esa condición debe aparecer después como finding/readiness y no convierte la Source
revision persistida en inválida.

## Invariantes full-revision CURRENT

Además de las validaciones locales de Core:

- `AlarmIdentity` única;
- `rule_name` único dentro de family;
- `priority_order` único dentro de `priority_group`;
- orden IMPACT/RISK del grupo;
- referencias Message deben existir en el mismo aggregate;
- Message FAMILY sólo puede referenciarse desde la misma family;
- referencias Special Condition deben existir;
- deben apuntar a `is_special_condition=true`;
- deben pertenecer a la misma family y `priority_group`.

## Tool references

Alarm Configuration almacena referencias por `tool_key` y, cuando corresponda,
Component/Subcomponent.

`tool_key` es identidad; `display_name` no lo es.

Una referencia externa no resuelta:

- no deshabilita la Rule;
- no elimina la Rule;
- no obliga a modificar la Alarm Source revision;
- bloquea únicamente las capacidades que realmente requieran esa resolución.

## Workflow administrativo CURRENT

Implementado sobre Manager generic:

```text
WORKSPACE
→ validate
→ verify Source
→ publish Source
→ project
→ history/preview
```

La validación Manager usa `AlarmConfiguration.from_document(...)`; la publicación vuelve a
reconstruir el aggregate antes de publicarlo.

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

Existe una superficie capability-local en modo documental para editar/importar el aggregate
completo sin crear otro contrato durable.

Permanece OPEN:

- editor visual especializado de Rules;
- Message Catalog UI final;
- editor visual genérico de parameters `str | float | bool`.

Estos refinamientos deben operar sobre el mismo `AlarmConfiguration` y no introducir un schema
paralelo.

## Configuration transversal

Dirección inicial:

- Profiles / permissions;
- Navigation.

No inicialmente:

- Tool editor;
- generic Actions module;
- User Activity;
- refresh configuration.
