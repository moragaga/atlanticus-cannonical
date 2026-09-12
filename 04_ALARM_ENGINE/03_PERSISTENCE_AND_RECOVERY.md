# Alarm Engine — Persistence and Recovery

Estado: **IMPLEMENTED + VALIDATED**

Implementación:
`scopes/ada-command-center/backend/alarms/persistence`

## Orden durable

La secuencia actual exige:

1. authority check;
2. validar/alinear head;
3. validar previous state;
4. abrir fenced mutation;
5. escribir/seal/append WAL;
6. publicar durable head;
7. materializar snapshots;
8. publicar materialized head.

Resumen:

`WAL -> DURABLE HEAD -> SNAPSHOTS -> MATERIALIZED HEAD`

No invertir esta secuencia sin nueva evidencia.

## Recovery

Si durable/materialized están desalineados, recovery es obligatorio.

Casos validados:

- crash después de WAL y antes de durable: descartar tail no durable;
- crash después de durable y antes de snapshot: replay exacto;
- crash durante múltiples snapshots: recuperación idempotente;
- snapshots escritos antes de materialized head: alinear/publicar;
- tail no durable: truncar;
- corrupción de durable/head: fail closed;
- snapshots sin durable authority: corrupción/fail closed.

## Invariante

Durable commit es la autoridad de replay. Un snapshot materializado no puede convertirse silenciosamente en autoridad superior al durable journal/head.
