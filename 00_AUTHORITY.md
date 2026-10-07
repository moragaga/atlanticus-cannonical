# Atlanticus — Authority

Estado: **CURRENT**

## Repositorios autoritativos

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- HEAD verificado para este cierre: `d97118d202dc1ea5ef3b0d1c18355d3a330824aa`
- Commit: `feat`
- Fecha del commit: `2026-10-07T01:34:06Z`

`atlanticus:main` es la realidad implementada actual.

Checkpoints relevantes de este hito Web:

```text
5c40faed4df7f3d7b6db79251144a9ec09e09e91
    KPI presentation stores can exist without KPI Delivery polling

569070fd1b9261f835cd0c2486af9b550bae241b
    Global Indicators collection-level Content State
    authoring/normal operational presentation contract

2253dc1e7ea591e21af4f08a19077fe87bfc2c36
    Global Indicator responsive corrections validated in IO

d97118d202dc1ea5ef3b0d1c18355d3a330824aa
    generic Global Indicator responsive/sizing ownership
    IO keeps only scope/policy-specific overrides
    ada-web-ui-global-indicator 0.2.10
    ada-generic-application 0.2.29
    ada-integrated-operations-application 0.1.2
```

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- HEAD inspeccionado antes de estos reemplazos: `af936617ebf4e04157e4ef905dd7bda598c1d05a`
- Fecha: `2026-10-07T00:34:09Z`

`atlanticus-cannonical:main` contiene el estado consolidado CURRENT.

Los archivos de este cierre son reemplazos candidatos. No se vuelven autoritativos hasta que el usuario los integre explícitamente.

### Decisions

- Repositorio: `moragaga/atlanticus-decisions`
- Rama: `main`
- HEAD inspeccionado: `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`
- Fecha: `2026-09-13T03:31:11Z`

`atlanticus-decisions` conserva decisiones, contratos, rationale y evidencia histórica. Una decisión vigente/frozen no debe ignorarse. Toda contradicción entre implementación, canonical y decisions debe registrarse como `CONFLICT`.

## Jerarquía de trabajo

```text
atlanticus:main
    realidad implementada

atlanticus-cannonical:main
    estado vigente consolidado

atlanticus-decisions:main
    decisiones/rationale/evidencia a contrastar
```

No resolver conflictos silenciosamente.

## Baseline técnico

Baseline objetivo del Project:

```text
Python 3.14.7
uv
python:3.14.7-slim-trixie
```

Los paquetes Web inspeccionados en este cierre siguen declarando Python `3.14.2`.

La migración Python 3.14.7 / Trixie permanece separada.

## Git

Git es **SOLO LECTURA** por defecto para el asistente.

No crear commits, push, branches, PR, issues ni otras mutaciones remotas sin autorización explícita.

## Forma de trabajo

```text
1. debate/diseño
2. implementación incremental tras consenso/autorización
```

Contratos antes que consumidores. Backend antes que frontend. Cambios de raíz reemplazan limpiamente soluciones superseded; no crear legacy, aliases, shims ni adapters temporales salvo decisión explícita.

## Estados y certeza

Certeza:

```text
VERIFIED
INFERRED
ASSUMED
PROPOSED
UNVERIFIED
```

Estado:

```text
CURRENT
IN PROGRESS
PLANNED
SUPERSEDED
BLOCKED
CLOSED
```
