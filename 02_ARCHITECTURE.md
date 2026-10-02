# Atlanticus — Architecture

Estado: **CURRENT**

## Regla principal

Atlanticus es una plataforma modular reusable.

ADA y ADA Command Center consumen Atlanticus.

El núcleo genérico de Atlanticus no depende de ADA ni de ADA Command Center.

## Ownership de Web distribution

```text
tooling/distribution/web
    shared distribution engine
    product catalog
    base starter

scopes/ada/tooling/distribution/web
    ADA distribution support
    ADA project tooling
    ADA starter overlay

scopes/ada-command-center/tooling/distribution/web
    Command Center starter overlay
```

Invariante:

```text
shared tooling
-X-> product runtime internals
```

Los productos pueden usar el motor compartido mediante contratos declarativos/handlers.

## Application ownership

### ADA

```text
ada-generic-application
    composition root
    runtime host/lifecycle
    Manager integration
    Tool Projection
    Master Projection
    KPI Collector attachment
```

### ADA Command Center

```text
ada-command-center-generic-application
    composition root

ada-command-center-configuration-manager
    reusable configuration/administration composition
    separate qualification/development application
```

No convertir Configuration Manager en servicio remoto ni en product root.

## Master Projection

Master Projection es una **extensión/runtime capability**, no una aplicación independiente y no tooling de distribución.

CURRENT ADA ownership:

```text
scopes/ada/web/application/ada-generic-application/
    .../generic/master_projection/
```

El Starter puede invocar contratos del product runtime, pero no duplicar reader/provisioning/lifecycle.

Command Center también requiere Master Projection por decisión de producto, pero esa integración aún está **PLANNED / NOT IMPLEMENTED**.

Si una capacidad resulta realmente reusable entre ADA y Command Center, extraer sólo el contrato genérico necesario; no hacer que Command Center dependa de `ada-generic-application`.

## Starter boundary

Un Starter existe para entregar un host editable/consumible.

```text
base starter
    minimal generic Atlanticus Web application

ADA starter
    host customization / composition extension / deployment surface

Command Center starter
    thin delegation to real product composition root
```

Un Starter no debe convertirse en una segunda implementación del runtime del producto.

## Wheelhouse portability

El wheelhouse compartido produce artefactos binarios instalables offline.

```text
locked compatible wheel
    → use directly

locked sdist when no compatible wheel exists
    → verify source SHA256
    → build wheel for current platform
    → hash-constrained build dependencies
    → record source + output hashes
```

No relajar hashes para resolver portabilidad.

## Application availability boundary

```text
APPLICATION EXISTENCE
!= CONFIGURATION EXISTENCE
!= INFRASTRUCTURE AVAILABILITY
!= DATA AVAILABILITY
```

La UI no debe asumir que ausencia inicial de datos equivale necesariamente a error.

## Python

CURRENT Web/distribution baseline:

```text
Python 3.14.2
```

Target histórico 3.14.7/Trixie permanece diferido.
