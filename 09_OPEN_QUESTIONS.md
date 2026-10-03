# Atlanticus — Open Questions

Estado: **CURRENT — ADA ALARM ENGINE EXTRACTION DESIGN NEXT**

## CLOSED

```text
Tool-scoped Source ownership
Global Users identity split
Tool User Membership
users-runtime
Tool Users Recovery Snapshot
Master Projection Users REPLACE
Command Center Users/Profiles/Navigation/Manager parity
Command Center Web lock normalization

KPI named connections
KPI Registry materialization
Latest multi-Tool delivery
Historian rolling projection
Timeseries multi-Tool delivery
KPI History dataset representation boundary
```

## OPEN / NEXT — ADA Alarm Engine extraction

El próximo chat debe resolver desde fuentes autoritativas:

```text
1. Confirmar si todo scopes/ada-command-center/backend pertenece conceptualmente al Alarm Engine.
2. Inventariar dependencias package por package.
3. Identificar dependencias backend -> Command Center Web.
4. Clasificar cada dependencia como KEEP / MOVE / REMOVE / INVERT / REHOME.
5. Definir el contrato exacto publicado por Command Center que consume Materialization.
6. Determinar el destino de ada-command-center/domain/alarms.
7. Definir el target físico scopes/ada-alarm-engine.
8. Definir renames/package ownership sin adapters ni re-exports legacy.
9. Definir orden incremental de extracción y qualification.
```

Working hypothesis:

```text
el backend actual es el Engine;
el problema principal de frontera son capas de adquisición/resolución
que aún dependen de Web.
```

Debe verificarse antes de mover código.

## BLOCKED / SEPARATE — Command Center full qualifier

El blocker de capability parity está cerrado.

El qualifier completo se detiene actualmente por incompatibilidad entre:

```text
ada.web.tools.*
ada.contracts.tools.*
```

No resolver dentro del frente Alarm Engine.

## OPEN / SEPARATE

```text
production Entra/Azure
artifact completeness/installability
.env.detail exhaustive audit
distribution regeneration
ADA durable end-to-end
runtime restart/readback
macOS rcssmin
/health/ready checks
Python migration
KPI operational E2E
Collector / Time Status / UI
Alarm Live Projection
History / Analytics
```
