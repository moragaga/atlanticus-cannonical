# Atlanticus — Authority

Estado: **CURRENT**

## Repositorios autoritativos

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- HEAD verificado para este cierre: `777f3a0894a58f7275473ab34ce6b33cf767f9e7`
- Fecha: `2026-10-05T19:17:18Z`

`atlanticus:main` es la realidad implementada actual.

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- HEAD inspeccionado antes de estos reemplazos: `44d3c803f60d1a1630d3a3374a663447cfe21248`
- Fecha: `2026-10-03T21:46:02Z`

`atlanticus-cannonical:main` contiene contratos, decisiones vigentes, qualification, rationale y estado canónico.

### Historical decisions

- Repositorio: `moragaga/atlanticus-decisions`
- Rama: `main`
- HEAD inspeccionado: `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`
- Fecha: `2026-09-13T03:31:11Z`

`atlanticus-decisions` es evidencia histórica. No prevalece sobre canonical vigente ni sobre implementación actual salvo referencia explícita.

En este cierre se localizaron artefactos históricos relacionados con Operational Data y KPI en formatos DOCX/XLSX, pero no pudieron inspeccionarse textualmente mediante el conector disponible. Su compatibilidad con este cierre permanece **UNVERIFIED**.

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

La implementación inspeccionada todavía conserva `requires-python = ==3.14.2` en workspaces relevantes. La migración 3.14.7/Trixie permanece separada y no fue parte de este hito.

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
