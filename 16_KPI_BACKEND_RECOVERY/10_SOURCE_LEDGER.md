# KPI Backend Reprocessing — Source Ledger

Estado: **AUDIT LEDGER**

Corte:
`moragaga/atlanticus@685924322c9cc0d625d112e25297a407f7a46acb`

Inspeccionado:

## KPI Runtime

`scopes/ada/backend/processes/kpi-runtime/.../job.py`

Verificado:
- source watermark;
- committed watermark;
- `up_to_date` skip;
- fenced commit.

## KPI Persistence

`scopes/ada/backend/kpis/persistence/.../commit.py`

Verificado:
- no backward watermark;
- same watermark permitido;
- `write_once`;
- durable conflict si contenido existente difiere.

## Latest Delivery

`scopes/ada/backend/processes/kpi-delivery/.../job.py`

Verificado:
- checkpoint;
- config revision;
- `SKIPPED_CURRENT`;
- committed evaluation batch authority.

## Historian

`scopes/ada/backend/processes/kpi-historian/.../job.py`

Verificado:
- historian authority;
- `SKIPPED_CURRENT`;
- `read_after(after, through)`.

`.../history.py`

Verificado:
- merge por stable keys;
- materialización por día;
- idempotent merge candidate para full rebuild.

## Timeseries Delivery

`scopes/ada/backend/processes/kpi-timeseries-delivery/.../job.py`

Verificado:
- Historian authority;
- aligned watermark;
- checkpoint/config revision;
- `SKIPPED_CURRENT`.
