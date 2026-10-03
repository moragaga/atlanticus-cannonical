# Atlanticus — Authority

Estado: **CURRENT**

## Repositorios autoritativos auditados

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Commit auditado: `6725237a19c4442fdfa1b32c3410c124e9348dbc`
- Fecha del commit: `2026-10-03T08:46:13Z`
- Mensaje: `feat`

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Commit de partida inspeccionado: `9a6dafce3382d2d21fa0ebf790af57daf8715e7a`
- Fecha del commit: `2026-10-03T05:54:56Z`

## Jerarquía congelada

1. `moragaga/atlanticus:main` es la realidad implementada actual.
2. `moragaga/atlanticus-cannonical:main` contiene decisiones, contratos, qualification, rationale y estado canónico vigente.
3. Tests, builds y logs reproducibles son evidencia de propiedades verificadas, pero no reemplazan implementación ni canonical.
4. Otros repositorios, documentos históricos, decisiones previas y conversaciones son referencias únicamente cuando se indiquen explícitamente. No tienen autoridad automática sobre Atlanticus.
5. Si implementación y canonical se contradicen, registrar el conflicto; no resolverlo silenciosamente.

`atlanticus-decisions` deja de ser autoridad vigente por defecto. Puede consultarse como evidencia histórica cuando el usuario lo indique, pero no prevalece sobre `atlanticus:main` ni `atlanticus-cannonical:main`.

## Baseline técnico CURRENT

```text
Python 3.14.2
uv
backend Python
Web Python + Dash + Flask + Gunicorn + JavaScript
Azure productivo / Docker local
Microsoft Entra ID como identidad objetivo
```

No migrar incidentalmente a Python 3.14.7 dentro de otro incremento.

## Git

Git es **SOLO LECTURA** por defecto para el asistente.

No crear commits, push, branches, PR, issues ni otras mutaciones remotas sin autorización explícita.

## Forma de trabajo

```text
1. debate/diseño
2. implementación incremental sólo tras consenso/autorización
```

Contratos antes que consumidores. Backend antes que frontend. Cambios de raíz reemplazan limpiamente soluciones superseded; no crear legacy, shims ni adapters temporales salvo decisión explícita.

## Estados y certeza

Usar:

```text
VERIFIED
INFERRED
ASSUMED
PROPOSED
UNVERIFIED
```

Y estado:

```text
CURRENT
IN PROGRESS
PLANNED
SUPERSEDED
BLOCKED
CLOSED
```
