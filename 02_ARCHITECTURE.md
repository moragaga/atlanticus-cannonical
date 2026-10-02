# Atlanticus — Architecture

Estado: **CURRENT**

## Regla principal

Atlanticus es una plataforma modular reusable.

ADA y ADA Command Center son productos/scopes consumidores.

El núcleo genérico de Atlanticus no depende de ADA ni de ADA Command Center.

## Web capabilities

Una capability reusable demostrada entre productos pertenece al área genérica Web.

CURRENT:

```text
web/capabilities/master-projection
    material
    reader
    planner
    executor
    independent Web surface
```

Product composition:

```text
ADA Generic
    -> product projection domains
    -> product provisioning/location policy

ADA Command Center Generic
    -> product projection domains
    -> product provisioning/location policy
```

No duplicar el motor Master Projection dentro de cada producto.

## Source

`SourceStore` y providers Local/Blob son genéricos:

```text
web/capabilities/source/core
web/capabilities/source/local
web/capabilities/source/blob
```

El contrato Core permanece frozen.

CURRENT gap de ownership:

```text
ada-command-center
    -> ada.web.storage.namespace
```

`AdaStorageNamespace` expresa actualmente:

```text
application namespace
sub-scope/tool namespace
local roots
Blob prefixes
```

Command Center lo reutiliza tratando `command-center` como segundo segmento.

Siguiente diseño debe determinar la forma genérica mínima de ese contrato sin reescribir
`SourceStore` ni hacer que Command Center dependa de ADA.

## Environment versus persistence

Congelado:

```text
ATLANTICUS_ENVIRONMENT
    local | production
    host/runtime behavior

persistence selector
    local | durable
    persistence topology
```

Emulator/Azure no son modos de arquitectura.

```text
LOCAL HOST + DURABLE PERSISTENCE
```

es una topología válida.

## Application ownership

### ADA

```text
ada-generic-application
    product composition root
    host/lifecycle
    Manager integration
    Tool Projection
    product Master Projection composition/provisioning
    KPI Collector attachment
```

### ADA Command Center

```text
ada-command-center-generic-application
    product composition root
    local host
    local/durable Manager selection
    product Master Projection composition/provisioning

ada-command-center-configuration-manager
    configuration/administration composition
    separate qualification/development application
```

Configuration Manager no es un servicio remoto ni product root.

## Tooling topology

CURRENT:

```text
/tooling
    generic/transversal mechanisms and orchestration

/scopes/ada/tooling
    ADA-specific distribution behavior

/scopes/ada-command-center/tooling
    Command Center-specific distribution behavior
```

PROPOSED / PLANNED, no implementado en este hito:

```text
/scopes/operational-data/tooling
/scopes/ada/tooling/distribution/backend
/scopes/ada-command-center/tooling/distribution/backend
```

Regla objetivo:

```text
scope tooling
    owns scope-specific build/distribution/qualification composition

root tooling
    owns reusable mechanisms + cross-scope orchestration
```

No mover lógica de producto al root y no usar Operational Data como contenedor genérico de tooling.

## Python

CURRENT Web:

```text
Python 3.14.2
```

Python 3.14.7/Trixie permanece diferido.
