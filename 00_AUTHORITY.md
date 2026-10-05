# Atlanticus — Authority

Estado: **CURRENT**

## Repositorios autoritativos

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- HEAD verificado para este cierre: `686a80f6a05eeea93d35d642cf2f92100cb1e61b`
- Fecha: `2026-10-05T19:12:27Z`
- Mensaje: `feat`

`atlanticus:main` es la realidad implementada actual.

### Decisiones

- Repositorio: `moragaga/atlanticus-decisions`
- Rama: `main`
- HEAD inspeccionado: `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`

`atlanticus-decisions:main` conserva decisiones, contratos, qualification, rationale y evidencia histórica.

Una decisión explícitamente vigente/frozen debe exponerse si contradice la implementación; no resolver el conflicto silenciosamente.

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- HEAD inspeccionado antes de estos reemplazos: `44d3c803f60d1a1630d3a3374a663447cfe21248`
- Fecha: `2026-10-03T21:46:02Z`
- Mensaje: `docs: reference integrated operations presentation baseline`

`atlanticus-cannonical:main` sintetiza el estado vigente para continuidad operativa. No sustituye silenciosamente una contradicción entre implementación y una decisión vigente.

## Jerarquía congelada

1. `moragaga/atlanticus:main`: realidad implementada.
2. Decisiones explícitamente vigentes/frozen en `moragaga/atlanticus-decisions:main`: intención contractual.
3. Canonical vigente: síntesis de estado, contratos e invariantes ya reconciliados.
4. Tests, logs reproducibles y qualification: evidencia de propiedades demostradas.
5. Otras referencias e historial conversacional: contexto, nunca autoridad suficiente por sí sola.

Si implementación, decisión y canonical se contradicen, registrar `CONFLICT`; no resolver silenciosamente.

## Referencias externas

Repositorios como `moragaga/isolated-web-functions` pueden utilizarse como referencia visual, funcional o histórica cuando el usuario lo indique.

No transfieren automáticamente:

```text
ownership
contratos
namespaces
dependencias
arquitectura
estado CURRENT
```

En el cierre ADA Web 2026-10-05, `isolated-web-functions/operational_trace` se utilizó exclusivamente como referencia de lenguaje visual para baseline/puntos. La arquitectura y los contratos CURRENT permanecen en Atlanticus.

## Baseline técnico

Baseline objetivo del Project:

```text
Python 3.14.7
uv
python:3.14.7-slim-trixie
```

Existe todavía código/package metadata en `atlanticus:main` que declara Python `3.14.2`.

Estado:

```text
PROJECT BASELINE  3.14.7 / CURRENT TARGET
PACKAGE METADATA  3.14.2 / IMPLEMENTED
CONFLICT          OPEN
```

No corregirlo incidentalmente dentro de otro incremento.

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
