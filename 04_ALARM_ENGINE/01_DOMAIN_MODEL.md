# Alarm Engine — Domain Model

Estado: **CURRENT — shared contract ownership migrated; business model frozen; physical Engine extraction PLANNED**.

## Modelo de negocio congelado

No reabrir por motivos de packaging:

- Rule;
- AlarmDefinition;
- AlarmIdentity `(family_key, alarm_key)`;
- PlannedAlarm;
- Occurrence;
- Episode;
- priority group/order;
- visibility;
- evaluator contract;
- routing/lifecycle/reappearance/deactivation semantics ya aceptadas.

## Shared contracts — CURRENT

### `ada-contracts-alarms==1.0.0`

Owner de contratos compartidos entre productos/consumidores:

```text
AlarmIdentity
AlarmKind
Criticality
Alarm definition/configuration value types
AlarmConfiguration
AlarmConfigurationSnapshot
Alarm configuration errors
Engine CURRENT/FACTS JSON schemas
```

Schemas CURRENT:

```text
ada/contracts/alarms/schemas/engine_committed_facts_batch.v1.schema.json
ada/contracts/alarms/schemas/engine_committed_facts_batch.v2.schema.json
ada/contracts/alarms/schemas/engine_resolved_current_state.v1.schema.json
```

### `ada-contracts-tools==1.0.0`

Owner de contratos Tool reutilizables:

```text
Tool enums
ToolStructure / component/subcomponent contracts
Tool source contracts
ToolDependencyEntry
ToolDependencyManifest
shared validation/errors
```

`ada-contracts-alarms` depende de `ada-contracts-tools`.

## Command Center domain — CURRENT

`scopes/ada-command-center/domain/alarms` ya no es owner de los modelos Alarm compartidos.

Su responsabilidad queda limitada a concern específico de Command Center:

```text
ALARM_CONFIGURATION_SOURCE_KEY
next_routing_tool_kind()
```

`scopes/ada-command-center/domain/tools` está **SUPERSEDED / REMOVED**.

## Engine internals — CURRENT

No mover a los packages de contracts:

```text
Planned/runtime state models
WAL / EFFECTIVE
RuntimeAlarmConfiguration
DeliveryAlarmConfiguration
materialization internal artifact types
leases/fencing/persistence internals
process bootstraps
```

Los packages de contracts no deben depender de ADA Web ni de Command Center.

## Authoring → Materialization contract — DECIDED

Command Center posee:

```text
authoring
semantic/business validation
Tool reference resolution
routing validation
visual target validation
publication
```

La publicación válida es:

```text
AlarmConfigurationSnapshot
    configuration
    ToolDependencyManifest
```

Un snapshot publicado debe ser materializable por contrato.

El stage adicional `ResolvedAlarmConfiguration` como contrato publicado está **SUPERSEDED**.

## Target de Materialization — PLANNED

```text
Published AlarmConfigurationSnapshot
        ↓
deterministic materialization
        ├── RuntimeAlarmConfiguration
        └── DeliveryAlarmConfiguration
```

Materialization no debe volver a:

- consultar Tool Catalog;
- comprobar existencia semántica de Tools;
- validar component/subcomponent membership de negocio;
- recalcular routing direction;
- volver a decidir visual target validity;
- rehacer qualification del evaluator.

Puede conservar validación mínima de integridad técnica/transport si existe una invariante real.

## Deuda CURRENT

La implementación física sigue bajo `scopes/ada-command-center/backend` y Materialization conserva dependencias/responsabilidades previas, incluyendo acoplamiento hacia superficies Web y resolución semántica. El cutover de packages no simplificó esa conducta.

No introducir adapter de compatibilidad. La futura corrección debe reemplazar esa frontera limpiamente.

## Extracción física

`ada-alarm-engine` permanece **PLANNED**.

Primero cerrar contratos y eliminar dependencias invertidas; luego evaluar mover físicamente Engine. Una frontera lógica no obliga a crear un servicio remoto.
