# Atlanticus — Authority

Estado: **CURRENT**

## Repositorios autoritativos

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- HEAD verificado para este cierre: `38379979fad90e2c514a2d56f3aa3889ceb71856`
- Fecha: `2026-10-03T17:58:26Z`
- Mensaje: `feat`

`atlanticus:main` es la realidad implementada actual.

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- HEAD inspeccionado antes de estos reemplazos: `8efd59431754059c548ed1e5d1263533b81012cd`
- Fecha: `2026-10-03T14:15:02Z`

`atlanticus-cannonical:main` contiene contratos, decisiones vigentes, qualification, rationale y estado canónico.

### Historical decisions

- Repositorio: `moragaga/atlanticus-decisions`
- Rama: `main`
- HEAD inspeccionado: `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`

`atlanticus-decisions` es evidencia histórica. No prevalece sobre canonical vigente ni sobre implementación actual salvo referencia explícita.

## Jerarquía congelada

1. `moragaga/atlanticus:main`: realidad implementada.
2. `moragaga/atlanticus-cannonical:main`: contratos y decisiones vigentes.
3. Tests, logs reproducibles y qualification: evidencia de propiedades demostradas.
4. `atlanticus-decisions` y otras fuentes históricas: contexto y provenance.
5. Historial conversacional: pista de búsqueda, nunca autoridad suficiente por sí sola.

Si implementación y canonical se contradicen, registrar `CONFLICT`; no resolver silenciosamente.

## Baseline técnico

Baseline general objetivo del Project:

```text
Python 3.14.7
uv
python:3.14.7-slim-trixie
```

Baseline CURRENT del workspace Alarm/Command Center implementado en este cierre:

```text
Python 3.14.2
uv
backend Python
Azure productivo / Docker local
```

La migración 3.14.7/Trixie permanece separada; no mezclarla incidentalmente con cambios Alarm.

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
