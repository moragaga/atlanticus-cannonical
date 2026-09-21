# ADA Command Center — Configuration Scope

Estado: **FROZEN DIRECTION / IMPLEMENTATION PENDING**

Command Center necesita Configuration propia porque es owner de Alarm Configuration.

No replica el Manager ADA como implementación paralela. La superficie administrativa debe reutilizar la capability genérica de Manager de Atlanticus para conservar simetría de workflow, estado y comportamiento, mientras el layout/editor específico de alarmas permanece bajo ownership de Command Center.

## NO Tool authoring

Command Center no crea/edita:

- Tool identity;
- Tool type;
- Components;
- Subcomponents;
- Tool topology.

Eso pertenece a Tool Configuration.

Command Center consume una vista reconciliada/read-only de esa topología mediante su Tool Catalog.

## Alarm Configuration aggregate

La unidad editable/publicable inicial contiene de forma atómica:

- Alarm Rules;
- Message Catalog.

El Tool Catalog no forma parte de este payload editable y tiene lifecycle/revisión independiente.

### Alarm Rules

Debe administrar el contrato ya congelado:

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
- stable message_key;
- display_text;
- active/inactive;
- optional deactivation override.

### Evaluator / parameters

`evaluator_key` identifica código de evaluator registrado por desarrollo.

Alarm Configuration no define schemas particulares por evaluator.

`parameters` conserva el contrato genérico:

```text
mapping[str, str | float | bool]
```

La validación intrínseca sólo exige keys válidas y valores de esos tres tipos.

Los nombres de parámetros, su significado y su uso pertenecen al evaluator/desarrollador.

La existencia del evaluator y el uso correcto de sus parámetros pertenecen a resolución/readiness/ejecución, no requieren crear modelos específicos dentro de Alarm Configuration.

Una configuración puede persistirse antes de que el evaluator o una Tool referenciada estén disponibles. Esa condición debe ser visible como finding de resolución y no convierte por sí sola la revisión persistida en inválida.

## Tool references

Alarm Configuration almacena referencias por `tool_key` y, cuando corresponda, Component/Subcomponent.

`tool_key` es identidad; `display_name` no lo es.

Distintas Tools tienen distintas `tool_key`, aunque compartan el mismo nombre visible.

Una referencia externa no resuelta:

- no deshabilita la Rule;
- no elimina la Rule;
- no obliga a modificar la Alarm Source revision;
- bloquea únicamente las capacidades que realmente requieran esa resolución.

## Workflow administrativo

La dirección es reutilizar el workflow genérico Manager:

```text
WORKSPACE
→ validate
→ verify Source
→ publish Source
→ project/materialize
→ history/preview
```

El botón de validación puede entregar feedback temprano, pero la publicación debe aplicar nuevamente la validación intrínseca autoritativa.

## Persistencia

Alarm Configuration adopta el contrato CURRENT de Source/Release de Atlanticus.

Para dominios migrados, Blob es el provider durable objetivo.

No crear una persistencia exclusiva de Command Center sin necesidad.

## Configuration transversal

Inicialmente:

- Profiles / permissions;
- Navigation.

No inicialmente:

- Tool editor;
- generic Actions module;
- User Activity;
- refresh configuration.
