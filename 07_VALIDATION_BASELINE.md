# Atlanticus — Validation Baseline

Estado: **CURRENT**

## Regla

Qualification y tests son evidencia de propiedades del contrato CURRENT.

No son autoridad para conservar contratos, schemas o adapters SUPERSEDED.

No inventar un PASS cuando no existe resultado de ejecución observado.

## Autoridad de implementación

```text
moragaga/atlanticus@ef3f0a44c5dcc14f8fcafe5bb36bb97865381924
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

KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
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

legacy token scan scoped
0 matches
```

Checkpoint publicado:

```text
4c7f8aa8b541e8b8f8abc7b49fe22526a4952bfe
```

## KPI Definition — evidencia observada

Qualification local ejecutada sobre el árbol que luego fue integrado:

```text
Python shell
3.14.7

uv lock
PASS

uv sync --group dev --extra web
PASS

uv run ruff check src tests
PASS

uv run pytest
40 passed

legacy token scan scoped over src/commented/tests
0 matches
```

El primer Ruff run detectó un único import no usado en `tests/test_web_runtime.py`; se eliminó ese import y la ejecución posterior pasó.

Los errores de `rm` sobre archivos legacy ya borrados no representaban fallo: `git status` confirmaba las eliminaciones esperadas.

Resultados bajo `build/lib` durante scans globales corresponden a artefactos generados y no son autoridad de código CURRENT. `build` está excluido del lint/package source relevante.

Checkpoint publicado e inspeccionado:

```text
ef3f0a44c5dcc14f8fcafe5bb36bb97865381924
```

Verificado por inspección:

```text
KpiDefinitionSourceService CURRENT
KpiDefinitionProjectionBuilder CURRENT
KpiDefinitionCatalog CURRENT
exact KPI Configuration ProjectionTarget dependency CURRENT
private authority/lifecycle/source/projection API removed
current tests present
```

## Criterio contractual de KPI Definition

```text
one generic Source contract
one generic Projection contract
exact KPI Configuration ProjectionTarget dependency
zero KpiDefinitionAuthority bridge
zero private Source/Projection lifecycle
zero private source revision identity
zero private projection revision identity
zero kpi_configuration_revision dependency identity
zero revision -> ProjectionTarget reconstruction
zero compatibility adapters inside KPI Definition
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
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
```

Luego ejecutar la regression completa y adjudicar sólo fallos del contrato final.

## UNVERIFIED

- Tools scoped pytest/Ruff posterior a su propio cutover;
- final Configuration Manager runtime sobre contrato genérico;
- full ADA suite posterior al cutover final;
- Docker E2E;
- CI remoto de `ef3f0a44...`;
- Python 3.14.7/Trixie global;
- concrete production provider composition para KPI destinations, si continúa siendo relevante tras el consumer cutover.

## Conflicto de metadata Python

El runtime observado durante qualification KPI Definition fue Python 3.14.7, pero los `pyproject.toml` publicados contienen:

```text
KPI Configuration requires-python = "==3.14.2"
KPI Definition    requires-python = "==3.14.2"
```

No considerar este conflicto resuelto por el hecho de que la suite scoped de KPI Definition haya pasado.

## Tests Web

La política CURRENT permanece:

```text
tests protect behavior/contracts
not CSS visuals
not existence/non-existence of internal functions/classes
not accidental module structure
```

La revisión transversal de tests Web queda:

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / AFTER MANAGER
```

No mezclarla con el cutover final del Configuration Manager salvo tests legacy directamente afectados por el contrato que se elimina.

## Git

Git continúa SOLO LECTURA para el asistente.
