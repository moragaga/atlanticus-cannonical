# KPI Backend Recovery — Testing

Estado: **VERIFIED for Materialization + Latest / TIMESERIES NEW DESIGN UNVERIFIED**

## Último hardening verificado

```text
kpi-materialization-runtime    14 passed
kpi-delivery-runtime           28 passed
Ruff                           PASS
format                         PASS
productive/commented mirrors   PASS
public imports                 PASS
wheel builds                   PASS
git diff --check               PASS
```

Gate previo también verificó:

```text
kpi-connections                 7 passed
```

## Qué acredita

VERIFIED:

```text
named connections parser
materialized Registry behavior
Materialization missing-Registry readiness
Latest materialization readiness
frozen process-lifetime configuration
Latest per-Tool checkpoint behavior
Latest parallel publication behavior
partial failure retry semantics
package imports/builds
commented semantic mirrors
```

## Política de tests refinada

No crear tests cuyo único objetivo sea:

```text
assert a word does not exist
assert a class/function does not exist
freeze internal implementation shape
freeze visual CSS/markup structure
```

Se eliminó el test que exigía ausencia del término `dispatch` y presencia explícita de `ThreadPoolExecutor`.

La concurrencia se valida por comportamiento.

Los boundaries se prueban solo cuando representan dependencia o contrato arquitectónico real.

## UNVERIFIED

```text
real Cosmos multi-Tool integration
real Azure credentials
RU/load profile
Historian rolling read model
new Timeseries hydration
new Timeseries multi-Tool parallel publication
new Timeseries per-Tool checkpoints
30 s Timeseries delivery output
```
