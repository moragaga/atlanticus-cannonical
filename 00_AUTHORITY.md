# Atlanticus — Authority

Estado: **CURRENT**

## Repositorios autoritativos auditados

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- HEAD inspeccionado: `09e9acf6edf6f84a66a4a0a041ad9a8f645daf79`
- Fecha del HEAD: `2026-10-03T13:56:26Z`
- Mensaje: `feat`

Para el frente Alarm Engine, la implementación relevante permanece sin cambios desde:

```text
346e7ac7ba7c21eede8b524613a6adee7e839e55
```

Verificación de cierre:

```text
compare 346e7ac7...09e9acf6
ahead_by = 4
```

Los cuatro commits posteriores modifican exclusivamente:

```text
scopes/ada-kpi-engine/*
backend/json/tests/*
tooling/gates/ada-kpi-engine/*
```

No modifican rutas de Alarm Engine / ADA Command Center Alarm backend.

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- HEAD inspeccionado antes de estos reemplazos: `f02b4740ca1002b060afdb94d142f2e2d8d588af`
- Fecha: `2026-10-03T12:00:57Z`

Estos archivos son reemplazos documentales preparados fuera de Git. Su existencia local no implica commit, push ni mutación remota.

## Jerarquía congelada

1. `moragaga/atlanticus:main` es la realidad implementada actual.
2. `moragaga/atlanticus-cannonical:main` contiene contratos, decisiones vigentes, qualification, rationale y estado canónico.
3. Tests y logs reproducibles son evidencia de propiedades verificadas, pero no reemplazan implementación ni canonical.
4. `atlanticus-decisions` y otros repositorios/documentos históricos son referencia sólo cuando se indiquen explícitamente.
5. Si implementación y canonical se contradicen, registrar el conflicto; no resolverlo silenciosamente.

## Baseline técnico CURRENT para este frente

```text
Alarm / Command Center packages actuales: Python 3.14.2
uv
backend Python
Web Python + Dash + Flask + Gunicorn + JavaScript
Azure productivo / Docker local
Microsoft Entra ID como identidad objetivo
```

Python 3.14.7 / Trixie permanece como migración separada. No introducirla incidentalmente dentro de la extracción Alarm Engine.

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
