# Atlanticus — Roadmap

Estado: **CURRENT ROADMAP — ALARM BACKEND LIVE VERTICAL CLOSED; WEB CONSUMER NEXT**

## CLOSED / CURRENT relevante

```text
COMMAND-CENTER-USERS-PROFILES-NAVIGATION-MANAGER-PARITY
COMMAND-CENTER-WEB-LOCK-NORMALIZATION

ALARM-CONFIGURATION-COSMOS-TO-MATERIALIZATION
ALARM-RUNTIME-EFFECTIVE-CURRENT-FACTS
ALARM-MODELER-BASELINE
ALARM-DELIVERY-LIVE-COSMOS
ALARM-BACKEND-LOCAL-E2E
```

## NEXT único

```text
ADA-COMMAND-CENTER-ALARM-LIVE-WEB-CONSUMER
```

Scope permitido:

```text
inspect current Web read surfaces
consume alarm-live-projection
map operator_view/operator_pool to existing logical UI surface
preserve Tool-scoped partition/identity
respect modeled slots/order
qualify read/render behavior
```

No recalcular prioridad, routing, eligibility ni scheduling en Web.

## PLANNED / SEPARATE

```text
full CAROUSEL/QIQ scheduler
Modeler durable scheduler state
Alarm Engine physical extraction
production qualification producer
History/Analytics
Management projections
resolution of ADA Tool contract duplication
current-head artifact/distribution qualification
.env.detail exhaustive audit
distribution regeneration
Python 3.14.7 / Trixie
production Azure / Entra
```

## BLOCKED / SEPARATE

```text
COMMAND-CENTER-FULL-WEB-QUALIFIER
```

Causa observada: coexistencia `ada.web.tools.*` vs `ada.contracts.tools.*`.
