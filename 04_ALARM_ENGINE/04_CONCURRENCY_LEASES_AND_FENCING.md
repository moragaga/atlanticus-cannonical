# Alarm Engine — Concurrency, Leases and Fencing

Estado: **IMPLEMENTED + VALIDATED**

## Propósito

Impedir que un writer que perdió autoridad complete una publicación parcial o sobrescriba al nuevo owner.

## Takeover después de WAL y antes de durable

Esperado:
- writer viejo queda bloqueado;
- durable head no avanza;
- nuevo owner recovery descarta tail no durable.

## Takeover después de durable y antes de snapshot

Esperado:
- writer viejo no materializa snapshot stale;
- durable commit ya publicado permanece;
- nuevo owner recovery reproduce exactamente ese commit.

## Mecanismos

La implementación actual usa authority/fencing checks en mutaciones y comparaciones de head para detectar stale/concurrent writers.

## Regla

Lease/fencing no es complejidad accidental. Existe por fallos de takeover/recovery ya sometidos a qualification.

No simplificar a “single worker normalmente” sin una nueva demostración que invalide estos riesgos.
