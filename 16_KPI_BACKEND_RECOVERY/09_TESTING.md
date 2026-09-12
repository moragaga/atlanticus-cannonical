# KPI Backend Reprocessing — Testing

Estado: **PROPOSED**

Tests de comportamiento, no de existencia de flags/functions.

## KPI Runtime

- current + flag false → skip.
- current + flag true + missing batch → rebuild.
- current + flag true + same batch → idempotent.
- current + flag true + conflicting batch → error.
- source regression → error aunque flag true.
- source missing → no source fabrication.

## Latest Delivery

- current + flag false → skip.
- current + flag true + missing snapshot → publish.
- current + flag true + existing identical snapshot → unchanged/idempotent.
- missing committed batch → error.

## Historian

- current + flag false → skip.
- current + flag true → read from beginning through committed.
- deleted history → rebuilt.
- existing history → merge idempotent.
- authority ahead of KPI → error.

## Timeseries

- current + flag false → skip.
- current + flag true → republish.
- missing historian authority → no fabricated result.

## Common

- cancellation respected;
- lease respected;
- fencing respected;
- facts indicate forced execution;
- default false.
