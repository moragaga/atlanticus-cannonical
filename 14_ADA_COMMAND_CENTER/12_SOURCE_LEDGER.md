# ADA Command Center — Source Ledger

Estado: **AUDIT LEDGER**

## `atlanticus:main`

Commit:
`685924322c9cc0d625d112e25297a407f7a46acb`

Inspeccionado:

- `scopes/ada-command-center/backend/alarms/core`
- `scopes/ada-command-center/backend/alarms/persistence`
- `scopes/ada-command-center/backend/processes/alarms-runtime`
- `definition.py`
- `journey.py`
- `evidence.py`

No existe `scopes/ada-command-center/web` en este corte.

## `atlanticus-decisions`

### B.1
`alarm_decisions/R3.6M-006B.1-alarm-definition-contract-inventory-DESIGN-FROZEN.md`

Estado:
`DESIGN FROZEN`

Producto:
`ADA Command Center`

### B.2
Decisiones de:
- Live vs Management Projection;
- publication/materialization;
- persistence gate;
- latest valid.

## Capabilities Atlanticus disponibles

`web/capabilities`:
- identity;
- manager;
- navigation;
- users.

Reutilizar selectivamente.

## Evidencia Web histórica

Existen prototipos/implementaciones históricas de dashboards de alarmas.

Son REFERENCIA, no autoridad de la Web nueva.

No portar automáticamente:
- CSS;
- geometría;
- polling;
- Redis assumptions;
- contratos snapshot antiguos.
