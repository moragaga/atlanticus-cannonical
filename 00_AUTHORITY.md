# Atlanticus — Authority

Estado: **CURRENT**

## Repositorios autoritativos auditados

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Commit auditado: `346e7ac7ba7c21eede8b524613a6adee7e839e55`
- Fecha del commit: `2026-10-03T11:41:18Z`
- Mensaje: `feat`

Este commit incorpora el cierre de paridad de ADA Command Center para Users / Profiles / Navigation / Manager y la normalización de los lockfiles Web alcanzados durante el qualifier.

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Commit de partida inspeccionado antes de estos reemplazos: `19fe30dcc2f34dbe7a0c4615c188409ace2089b8`
- Fecha del commit: `2026-10-03T09:11:10Z`

## Jerarquía congelada

1. `moragaga/atlanticus:main` es la realidad implementada actual.
2. `moragaga/atlanticus-cannonical:main` contiene contratos, decisiones vigentes, qualification, rationale y estado canónico.
3. Tests y logs reproducibles son evidencia de propiedades verificadas, pero no reemplazan implementación ni canonical.
4. `atlanticus-decisions` y otros repositorios/documentos históricos son referencia sólo cuando se indiquen explícitamente.
5. Si implementación y canonical se contradicen, registrar el conflicto; no resolverlo silenciosamente.

## Baseline técnico CURRENT

```text
Python 3.14.2
uv
backend Python
Web Python + Dash + Flask + Gunicorn + JavaScript
Azure productivo / Docker local
Microsoft Entra ID como identidad objetivo
```

Python 3.14.7 / Trixie permanece como migración separada. No introducirla incidentalmente dentro de otro incremento.

## Git

Git es **SOLO LECTURA** por defecto para el asistente.

No crear commits, push, branches, PR, issues ni otras mutaciones remotas sin autorización explícita.

## Forma de trabajo

```text
1. debate/diseño
2. implementación incremental sólo tras consenso/autorización
```

Contratos antes que consumidores. Backend antes que frontend. Cambios de raíz reemplazan limpiamente soluciones superseded; no crear legacy, aliases, shims ni adapters temporales salvo decisión explícita.

## Estados y certeza

Usar:

```text
VERIFIED
INFERRED
ASSUMED
PROPOSED
UNVERIFIED
```

Y:

```text
CURRENT
IN PROGRESS
PLANNED
SUPERSEDED
BLOCKED
CLOSED
```
