# Atlanticus — Open Questions

Estado: **CURRENT — WEB ALARM SURFACE FOUNDATION IS NEXT FOR THIS TRACK**

## CLOSED — Web presentation

```text
Tool READY materializes KPI presentation stores without Delivery
Collector polling separated from store existence
Global Indicator collection-level Content State
AUTHORING/NORMAL same real composition
Global Indicator generic responsive/sizing ownership
IO-specific MINE/PLANT policy retained in IO
```

## OPEN / NEXT — Web alarm surface

El próximo chat de este track debe resolver exclusivamente:

```text
1. What is the minimal reusable contract for the first alarms-flow component/card?
2. Which existing live/read-model contract feeds that card?
3. How are alarm-management and alarm-status mounted in the operational experience without leaking engine semantics into UI?
4. What header sizing is correct once branding + GI + alarm-management + alarm-status coexist?
5. Which behavior belongs to generic Web components and which remains product-specific?
6. What focused behavior tests prove the card/integration without freezing CSS layout?
```

## OPEN / NON-BLOCKING SMOKE

```text
final mobile/desktop GI visual smoke after generic refactor
final MINE/PLANT browser smoke after generic refactor
final KPI Inspection browser smoke after generic refactor
```

These are validation carryovers, not a new architecture front.

## OPEN / AFTER — Alarm Engine migration

Do not mix with the next Web increment.

Questions to resolve in the later engine-design chat:

```text
1. What packages/classes currently own operational alarm execution?
2. Which contracts remain in ada-contracts-alarms?
3. Which operational contracts move into ada-alarm-engine?
4. Which current fields have no demonstrated consumer/domain value?
5. Which fields are frozen by decisions and therefore cannot be removed without an explicit superseding decision?
6. How does Alarm Runtime migrate to DataInputSpec/DataInputContext?
7. Which lifecycle/persistence/modeler/delivery boundaries remain unchanged?
8. What clean cutover removes the historical engine owner without a compatibility layer?
```

Target direction:

```text
ada-alarm-engine
```

is PLANNED, not yet implemented.

## OPEN / PARALLEL — Distribution and platform

The existing independent track remains:

```text
extension-resource integration qualification
artifact generation/qualification
.env.detail exhaustive audit
distribution qualification
Python 3.14.7 / Trixie
production Azure / Entra
```

No treat this Web closure as superseding those fronts.
