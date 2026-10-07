# Atlanticus — Authority

Estado: **CURRENT**

## Repositorios autoritativos

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- HEAD verificado para este cierre: `5c40faed4df7f3d7b6db79251144a9ec09e09e91`
- Commit: `feat`
- Fecha del commit: `2026-10-07T00:11:14Z`

`atlanticus:main` es la realidad implementada actual.

Checkpoint relevante de este hito:

```text
5c40faed4df7f3d7b6db79251144a9ec09e09e91
    dynamic consumer-owned deployment resources
    Docker Compose resource override at execution time
    simulation using the same resource contract
    process integration resource initialization
    local Docker runtime-input contract alignment
    process-deployment gate requalification
```

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- HEAD inspeccionado antes de estos reemplazos: `15a51f70396726a2ad3b88d1afc66ce8cfff3300`
- Fecha: `2026-10-06T17:17:17Z`

`atlanticus-cannonical:main` contiene el estado consolidado CURRENT para continuar trabajo.

Los archivos generados por este cierre son reemplazos candidatos. No se vuelven autoritativos hasta que el usuario los integre explícitamente.

### Decisions

- Repositorio: `moragaga/atlanticus-decisions`
- Rama: `main`
- HEAD inspeccionado: `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`
- Fecha: `2026-09-13T03:31:11Z`

`atlanticus-decisions` conserva decisiones, contratos, rationale y evidencia histórica. Una decisión explícitamente vigente/frozen no debe ignorarse; cualquier contradicción con implementación o canonical debe registrarse como `CONFLICT`.

## Jerarquía de trabajo

```text
atlanticus:main
    realidad implementada

atlanticus-cannonical:main
    estado vigente consolidado

atlanticus-decisions:main
    decisiones/rationale/evidencia que deben contrastarse
```

No resolver conflictos silenciosamente.

## Baseline técnico

Baseline objetivo del Project:

```text
Python 3.14.7
uv
python:3.14.7-slim-trixie
```

La implementación de process deployment inspeccionada en este cierre sigue usando Python `3.14.2`.

La migración Python 3.14.7 / Trixie continúa como frente separado.

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
