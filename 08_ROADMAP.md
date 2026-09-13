# Atlanticus — Roadmap

Estado: **CANONICAL BASELINE 1.0**

## Regla

El bootstrap arquitectónico se considera cerrado para comenzar ejecución.

Los puntos aún no definidos quedan como `OPEN` y se resuelven cuando el primer Golden Path los necesite.

No continuar expandiendo arquitectura general antes de avanzar producto.

## PRIORIDAD 1 — Operaciones Integradas

Primera Tool real.

Cerrar verticalmente:

1. Tool Configuration;
2. Component contracts;
3. data/collector mapping;
4. KPI;
5. Alarm integration;
6. ADA Generic;
7. Manager;
8. artifact/distribution.

## PRIORIDAD 2 — Mina

Repetir el patrón estabilizado con Operaciones Integradas.

## Web Platform

En paralelo sólo los enablers que bloquean la vertical:

- Login/Bootstrap Console;
- base projections;
- derived resolutions;
- resource readiness;
- Manager authorization sin bypass.

Resource readiness / Users durable chain tiene el siguiente checkpoint:

```text
WEB-STORAGE-TOPOLOGY              CLOSED / VERIFIED / CURRENT
USERS-STORAGE-TOPOLOGY            CLOSED / VERIFIED / CURRENT
STORAGE-PREFLIGHT-COSMOS-BRIDGE   CLOSED / VERIFIED / CURRENT
COSMOS-USERS-RUNTIME-ADAPTER      CLOSED / VERIFIED / CURRENT
USERS-RUNTIME-PROJECTION-BOUNDARY PLANNED / NEXT
```

Bridge y adapter permanecen fronteras separadas: el bridge traduce/prepara topology provider-specific; el adapter implementa lectura/observación del runtime Users. Ninguno define por sí solo el writer administrativo de Managed Users.

Siguiente foco aislado: auditar y congelar el contrato que materializa Users Configuration/Projection hacia `users.runtime`. No mezclarlo con el lifecycle global de resource readiness.

## Backend productization

Cerrar:

- `REPROCESS_CURRENT`;
- process generation;
- artifact/distribution contract;
- component scripts;
- master integrity check;
- env.detail;
- READMEs.

## Frontend productization

Cerrar:

- generated ADA Generic application;
- distributable artifact;
- loaders;
- readiness/bootstrap integration;
- alarm management consuming backend contract;
- ADA Component External Links.

## Command Center

Primero:

1. Web/configuration foundation;
2. Alarm B.2 integration;
3. History/Data Sufficiency qualification.

No implementar analytics todavía.

Después de data sufficiency GREEN:

```text
History read model
→ analytics
→ dashboard/storytelling
```

## Infrastructure inventory

No congelar la inventory global de containers hasta disponer de traza completa de recursos usados por Operaciones Integradas y las capabilities asociadas.

`users.runtime` sí queda confirmado individualmente y no implica que la inventory global esté cerrada.

## University

Crear casos pequeños a medida que una capability queda estable.

No esperar al final para escribirlos todos.

## Delivery boundary

Atlanticus produce:

```text
validated source
→ artifact
→ distribution-ready package
```

No es owner del pipeline corporativo DevOps.
