# ADA Command Center — Domain Ownership and Migration

Estado: **CURRENT — shared contracts extracted; capability parity closed; Alarm Engine physical extraction PLANNED/NEXT DESIGN**.

Checkpoint:

```text
atlanticus@346e7ac7ba7c21eede8b524613a6adee7e839e55
```

## Shared Tool contracts — CURRENT

```text
scopes/ada-contracts/tools
ada-contracts-tools==1.0.0
```

Owns reusable Tool contracts.

`scopes/ada-command-center/domain/tools` permanece SUPERSEDED / REMOVED.

## Shared Alarm contracts — CURRENT

```text
scopes/ada-contracts/alarms
ada-contracts-alarms==1.0.0
```

Owns reusable Alarm configuration models/snapshot/errors and Engine publication schemas.

## Command Center Alarm domain — CURRENT physical state

```text
scopes/ada-command-center/domain/alarms
```

Actualmente contiene policy residual como:

```text
ALARM_CONFIGURATION_SOURCE_KEY
next_routing_tool_kind()
```

Su destino durante la extracción del Engine sigue OPEN y debe clasificarse como REHOME / KEEP / REMOVE según ownership real.

## Command Center Web — CURRENT ownership

Command Center conserva:

```text
Alarm authoring
semantic validation
Tool reference validation
routing/visual validation
publication
Manager/Web surfaces
```

## Backend Engine — CURRENT physical location

```text
scopes/ada-command-center/backend/alarms/core
scopes/ada-command-center/backend/alarms/materialization
scopes/ada-command-center/backend/alarms/persistence
scopes/ada-command-center/backend/processes/alarms-materialization
scopes/ada-command-center/backend/processes/alarms-runtime
scopes/ada-command-center/backend/processes/alarms-delivery
```

## Working hypothesis — PROPOSED / NEXT

Todo el backend anterior pertenece conceptualmente al Alarm Engine.

La extracción no debe seleccionar sólo `core`; debe evaluar el backend completo como unidad.

Las dependencias que hoy apuntan desde backend a Web son deuda de frontera y deben removerse o invertirse, no preservarse mediante adapters.

## Known backend → Web debt

`alarms-materialization-process` declara actualmente dependencias hacia superficies Web, incluyendo:

```text
ada-command-center-web-alarm-configuration
ada-command-center-web-alarm-projection-cosmos
atlanticus-web-projection
atlanticus-web-source
```

Esto no se considera target architecture.

## Target direction — PROPOSED

```text
Command Center
    authoring + validation + resolution + publication
        ↓
published shared Alarm contract
        ↓
ada-alarm-engine
    core
    materialization
    persistence
    runtime
    delivery
```

Engine no debe importar Command Center Web.

## Materialization target

El snapshot publicado debe ser suficiente para materializar sin:

```text
Tool rediscovery
Command Center Web imports
second semantic validation pass
```

`ResolvedAlarmConfiguration` como stage adicional permanece SUPERSEDED salvo nueva decisión explícita.

## Qualifier

Capability parity ya no bloquea el qualifier.

Permanece un bloqueo separado por coexistencia de contratos Tools de ADA; no forma parte de la extracción del Engine.

## No legacy

No crear:

```text
re-export packages
temporary aliases
backend -> old Command Center wrappers
parallel engine copies
```

El movimiento físico debe reemplazar limpiamente ownership y package names una vez congelado el diseño.
