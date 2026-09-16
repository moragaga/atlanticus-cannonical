# Atlanticus — Validation Baseline

Estado: **CURRENT**

## Regla

Qualification y tests son evidencia de propiedades del contrato CURRENT.

No son autoridad para conservar contratos, schemas o adapters SUPERSEDED.

No inventar un PASS cuando no existe resultado de ejecución observado.

## Autoridad de implementación

```text
moragaga/atlanticus@4c7f8aa8b541e8b8f8abc7b49fe22526a4952bfe
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

KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## Evidencia anterior conservada — Users

La qualification global documentada anterior permanece atribuida únicamente a su checkpoint correspondiente:

```text
ruff scoped
PASS

pytest scoped
99 passed

full Web pytest
545 passed
7 skipped
0 failed
```

No trasladar esa evidencia a checkpoints posteriores.

## Tools — ejecución

La implementación Tools Source/Projection permanece CLOSED / VERIFIED / CURRENT por inspección.

Su ejecución scoped posterior a su cutover continúa UNVERIFIED en esta documentación.

## KPI Configuration — evidencia observada

Qualification local ejecutada sobre el árbol que luego fue integrado:

```text
uv lock
PASS

uv sync --group dev
PASS

uv run ruff check src tests
PASS

uv run pytest
45 passed

git diff --check
PASS
```

Después de limpiar `.pytest_cache` y `build`, la búsqueda scoped en:

```text
src
commented
tests
```

para los tokens legacy acordados devolvió:

```text
0 matches
```

Tokens verificados:

```text
tool_projection_revision
source_revision
projection_revision
expected_source_revision
build_kpi_configuration_digest
KpiConfigurationSourceDocument
KpiConfigurationProjectionWorkflow
```

El shell de qualification mostró Python 3.14.7.

## KPI Configuration — inspección publicada

Checkpoint CURRENT:

```text
4c7f8aa8b541e8b8f8abc7b49fe22526a4952bfe
```

Verificado por inspección:

```text
KpiSourceService added/current
KpiProjectionBuilder added/current
generic Source dependency added
generic Projection dependency added
private lifecycle/source/projection files removed
legacy-only tests removed
current tests present
```

## Criterio contractual de KPI Configuration

```text
one generic Source contract
one generic Projection contract
exact Tool ProjectionTarget dependency
zero private lifecycle contract
zero source revision identity
zero private projection revision identity
zero tool_projection_revision dependency identity
zero revision -> ProjectionTarget reconstruction
zero compatibility adapters inside KPI Configuration
ADA ownership preserved
```

Resultado:

```text
IMPLEMENTED / VERIFIED IN MAIN
SCOPED LOCAL QUALIFICATION / PASS
```

## Regresión final de Configuration

Se difiere hasta completar:

```text
KPI Definition Source/Projection
ADA Configuration Manager final cutover
```

Luego ejecutar la regression completa y adjudicar sólo fallos del contrato final.

## UNVERIFIED

- Tools scoped pytest/Ruff posterior a su propio cutover;
- full ADA suite;
- final Configuration Manager runtime;
- Docker E2E;
- CI remoto de `4c7f8aa8...`;
- Python 3.14.7/Trixie global;
- concrete production provider composition para KPI destinations;
- KPI Definition.

## Conflicto de metadata Python

El runtime observado en qualification fue Python 3.14.7, pero el `pyproject.toml` publicado de KPI Configuration todavía contiene:

```text
requires-python = "==3.14.2"
```

No considerar este conflicto resuelto por el hecho de que la suite haya pasado.

## Git

Git continúa SOLO LECTURA para el asistente.
