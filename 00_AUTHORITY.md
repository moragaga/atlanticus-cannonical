# Atlanticus — Authority

Estado: **CURRENT**

## Repositorios autoritativos

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- HEAD verificado para este cierre: `6ecbfb21dd0f98d7cae8f0c142d796994a7fc361`
- Fecha: `2026-10-05T21:45:17Z`

`atlanticus:main` es la realidad implementada actual.

Checkpoints relevantes de este hito:

```text
a334d14b1e0481a04676492c2daa3bf8aa436380
    additive AdaApplicationExtension contract

2ddb968c79d203a5e6bdc2acf6ec6da1e36dc9c6
    AdaApplicationDescriptor
    ada-integrated-operations-application foundation

6ecbfb21dd0f98d7cae8f0c142d796994a7fc361
    Integrated Operations local emulator harness
    product-owned resource preparation entry point
```

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- HEAD inspeccionado antes de estos reemplazos: `ed26b441b3055e582ba272e349f8cd6de0e067fa`
- Fecha: `2026-10-05T19:26:15Z`

`atlanticus-cannonical:main` contiene contratos, decisiones vigentes, qualification, rationale y estado canónico.

### Historical decisions

- Repositorio: `moragaga/atlanticus-decisions`
- Rama: `main`
- HEAD inspeccionado: `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`
- Fecha: `2026-09-13T03:31:11Z`

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

La implementación inspeccionada todavía conserva `requires-python = ==3.14.2` en workspaces relevantes, incluido `ada-integrated-operations-application`.

La migración 3.14.7/Trixie permanece separada y no fue parte de este hito.

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
