# Atlanticus — Validation Baseline

Estado: **CURRENT**

## Regla

Qualification y tests son evidencia de propiedades del contrato CURRENT.

No son autoridad para conservar contratos, schemas o adapters SUPERSEDED.

No inventar un PASS cuando no existe resultado de ejecución observado.

## Autoridad de implementación

```text
moragaga/atlanticus@27c2e4beed125fe379881048f0df5fbe3ff6cb1a
```

## Hitos contractuales

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-MANAGER-GENERIC-CONTRACT-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-CLEAN-CUTOVER-COMPLETION
CLOSED / VERIFIED / CURRENT

USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL
CLOSED / VERIFIED / CURRENT

PROJECTION-CORE-STALE-TEST-ALIGNMENT
CLOSED / VERIFIED

TOOLS-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## Evidencia anterior conservada — Users

La última qualification global documentada antes de Tools permanece:

```text
ruff scoped
PASS

pytest scoped
99 passed

full Web pytest
545 passed
7 skipped
0 failed

git diff --check HEAD^..HEAD
PASS

git status --short
CLEAN
```

Esa evidencia corresponde al checkpoint anterior y no debe atribuirse a `27c2e4be...`.

## Tools — evidencia CURRENT

VERIFIED por inspección del commit:

```text
private Tool lifecycle files removed
private Tool source/projection snapshots removed
ToolSourceService added
ToolProjectionBuilder added
SourceProjectionService[ToolConfiguration] used
new CURRENT tests added:
  test_source_release.py
  test_source_projection.py
legacy-only tests removed
```

## Tools — ejecución

```text
uv run pytest scoped
UNVERIFIED

Ruff scoped
UNVERIFIED

CI remoto
UNVERIFIED / no status observado
```

No declarar PASS hasta observar ejecución real.

## Política de tests vigente

```text
DO
- fijar contrato final;
- eliminar legacy;
- eliminar/reemplazar tests del contrato eliminado;
- escribir tests del comportamiento CURRENT;
- ejecutar qualification en la frontera acordada;
- ejecutar regression global después del cutover final de Configuration Manager.

DO NOT
- conservar adapters para salvar tests;
- mantener consumers viejos funcionando mediante aliases;
- adaptar producción al contrato retirado;
- considerar la existencia de tests como equivalente a haberlos ejecutado.
```

## Criterio contractual de Tools

```text
one Source contract
one Projection contract
zero private lifecycle contract
zero Tool source revision identity
zero private projection revision identity
zero revision -> ProjectionTarget reconstruction
zero compatibility adapters inside Tools
Tool domain semantics preserved
```

Resultado por inspección:

```text
IMPLEMENTED / VERIFIED IN MAIN
```

Resultado de ejecución:

```text
UNVERIFIED
```

## Regresión final de Configuration

Se difiere hasta completar:

```text
KPI Configuration Source/Projection
KPI Definition Source/Projection
ADA Configuration Manager final cutover
```

Luego ejecutar la regression completa y adjudicar sólo fallos del contrato final.

## UNVERIFIED

- Tools scoped pytest;
- Tools scoped Ruff;
- full ADA suite;
- final Configuration Manager runtime;
- Docker E2E;
- CI remoto;
- Python 3.14.7/Trixie global;
- KPI Configuration;
- KPI Definition.

## Git

Git continúa SOLO LECTURA para el asistente.
