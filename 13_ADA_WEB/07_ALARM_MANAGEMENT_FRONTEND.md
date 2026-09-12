# ADA Web — Alarm Management Frontend

Estado: **CURRENT DIRECTION**

## Regla

Frontend implementa gestión de alarmas **en base al contrato que devuelve Backend**.

No reimplementa:

- priority;
- capabilities;
- deactivation eligibility;
- management rules;
- Message resolution;
- lifecycle.

## Backend payload

La proyección debe entregar suficiente información para:

- identificar occurrence;
- mostrar Rule/context;
- mostrar management/deactivation state;
- conocer actions/capabilities disponibles;
- mostrar mensajes;
- ejecutar acción mediante contrato backend.

## Frontend

Responsabilidad:

- presentar;
- solicitar acción;
- mostrar resultado;
- mantener UX estable;
- actualizar desde nueva proyección.

## No hidden business logic

No usar callbacks/UI conditions para decidir si una acción "debería" estar permitida cuando Backend ya es authority.

El front puede deshabilitar anticipadamente según capabilities devueltas, pero Backend vuelve a validar.

## Sequence

```text
Alarm Engine / Projection contract
        ↓
freeze management payload/actions
        ↓
ADA Web management UI
        ↓
E2E qualification
```
