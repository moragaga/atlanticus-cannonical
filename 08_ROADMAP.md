# Atlanticus — Roadmap

Estado: **CURRENT ROADMAP — PARALLEL FRONTS EXPLICIT; WEB ALARM SURFACE NEXT IN THIS TRACK**

## CLOSED / CURRENT — Web operational track

```text
KPI-PRESENTATION-STORES-WITHOUT-DELIVERY
GLOBAL-INDICATOR-COLLECTION-CONTENT-STATE
GLOBAL-INDICATOR-AUTHORING-NORMAL
GLOBAL-INDICATOR-GENERIC-RESPONSIVE
IO-MINE-PLANT-GI-POLICY
```

## NEXT único — Web operational track

```text
ADA-WEB-ALARM-SURFACE-FOUNDATION
```

Orden acotado:

```text
1. confirm post-refactor GI browser smoke
2. define the first reusable component/card contract for alarms flow
3. implement one alarm card/component
4. integrate alarm-management into the operational experience
5. integrate alarm-status into the operational experience
6. exercise branding + GI + alarm-management + alarm-status together
7. adjust header proportions only with all four surfaces present
8. freeze Web alarm-consumption boundary
```

No mezclar en este incremento:

```text
Alarm Engine migration
AlarmDefinition cleanup
History/Analytics
large component catalog build-out
distribution/tooling parallel work
Python migration
Azure production qualification
```

Una vez congelado el primer patrón de componente/card, el usuario puede avanzar de forma incremental en más componentes sin reabrir la arquitectura.

## AFTER — Alarm Engine track

```text
ADA-ALARM-ENGINE-MIGRATION
```

Primera etapa obligatoria:

```text
inventory current alarm packages
inventory shared contracts
inventory current runtime/data-input break
compare against frozen alarm decisions
identify fields with no demonstrated value
freeze keep/move/remove decisions
```

Luego:

```text
clean target package: ada-alarm-engine
migrate Operational Data consumption
preserve lifecycle/projection boundaries
qualify engine
connect to prepared Web surfaces
```

No crear legacy adapters para mantener el engine histórico.

## Parallel Distribution/Tooling track

La hoja de ruta previa de Distribution/Tooling continúa en su chat/scope propio.

Este cierre Web no supersede:

```text
EXTENSION-RESOURCE-INTEGRATION-QUALIFICATION
artifact/env-detail qualification
distribution qualification
```

## PLANNED / SEPARATE

```text
History/Analytics
Python 3.14.7 / Trixie
production Azure / Entra
```
