# ADA Web — Current Baseline

Estado: **CURRENT**

## Implementación auditada

```text
moragaga/atlanticus@6fd1512afed73e76f7c344f3acb989b601c453e3
```

## Manager authorization

```text
CLOSED / VERIFIED / CURRENT
```

Manager Core implementa `administrative_override` y `manager_access_granted`.

ADA Generic compone:

```text
managed root                         → override
trusted local + local environment    → override
ordinary profiles                    → no override
bootstrap root                       → no implicit override
```

ADA Access no es autoridad de permisos Manager.

## ADA Configuration Manager

Los checks de acciones administrativas convergen con la autorización genérica del Manager.

El runtime local usa override administrativo y no una lista enumerada de módulos.

`MANAGER_ACCESS_KEYS` fue retirado.

## KPI Registry / Definition / Collector

```text
CURRENT capabilities
```

Collector conserva:

```text
Latest polling      10 s
Timeseries polling 120 s
Browser refresh     10 s
1 ToolComponent = 1 browser store
```

Tool Projection y KPI Delivery continúan siendo fronteras separadas.

## ADA Generic operational bootstrap

```text
AdaGenericSettings
→ ToolPersistenceComposition
→ durable Tool Projection resolution
→ WebApplicationDefinition
```

Estados:

```text
READY
UNCONFIGURED
UNAVAILABLE
INVALID
```

## Tooling baseline

`ToolConfiguration` mantiene configuración, sources, participación operacional, estructura y branding.

`ToolStructure` expone destinos KPI y estructura consumible por contratos Alarm baseline.

La consolidación Tool→Tool permanece **OPEN / UNVERIFIED**; no se define aquí un schema.

## Qualification de este cierre

```text
ada-generic-application             288 passed
ruff check src tests                PASS
ada-configuration-manager tests      70 passed
manager core tests                   85 passed
MANAGER_ACCESS_KEYS search            0 matches
git diff --check                     PASS
```

No equivale a qualification completa de Docker, Azure, Entra, multiworker o todo el monorepo.

## Próximo foco

```text
ADA-TOOLING-CONTRACT-REVIEW
PLANNED / NEXT
```

Luego:

```text
ADA-END-TO-END-GOLDEN-PATH
```
