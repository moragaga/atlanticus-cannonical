# ADA Command Center — Configuration Scope

Estado: **CANDIDATE**

Command Center necesita Configuration propia porque es owner de Alarm Configuration.

No necesita replicar todo el Manager ADA.

## NO Tool authoring

Command Center no crea/edita:

- Tool identity;
- Tool type;
- Components;
- Subcomponents;
- Tool topology.

Eso pertenece a Tool Configuration.

Command Center consume esa topología como dependencia confirmada/read-only.

## Configuración inicial

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

La UI debe conocer evaluators válidos y sus parámetros.

No permitir:
- código ejecutable;
- estructuras arbitrarias;
- parámetros inventados sin contrato.

La fuente exacta de metadata/schema del evaluator queda por congelar.

### Configuration transversal

Inicialmente:
- Profiles / permissions;
- Navigation.

No inicialmente:
- Tool editor;
- generic Actions module;
- User Activity;
- refresh configuration.

## Persistencia

Alarm Configuration debe adoptar el Source/Release común de Atlanticus cuando `SourceStore` quede congelado.

No crear una persistencia exclusiva de Command Center sin necesidad.
