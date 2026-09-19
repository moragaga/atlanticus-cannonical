# Atlanticus — Validation Baseline

Estado: **CURRENT**

## Regla

Qualification y tests son evidencia de propiedades del contrato CURRENT.

No son autoridad para conservar contratos, schemas, adapters o aliases SUPERSEDED.

No inventar un PASS cuando no existe resultado observado.

## Autoridad de implementación

```text
moragaga/atlanticus@fbef06a8a0a587571527d9ecf131c73c5fc5f01a
```

Parent:

```text
9f12c41a23d69784c7c5b775a4093a94ac654d55
```

Tree:

```text
fc8c293f617aca4a53d89f687a22728e9d0fdcca
```

## Hitos contractuales relevantes

```text
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
CLOSED / VERIFIED / CURRENT

MANAGER-AUTHORIZATION-SEMANTICS-ALIGNMENT
CLOSED / VERIFIED / CURRENT

MANAGER-ACTIVE-WORKFLOW-CALLBACK-CARDINALITY
CLOSED / VERIFIED / CURRENT

CONFIGURATION-UI-COMPOSITION-RECOVERY
CLOSED / VERIFIED / CURRENT

GENERIC-WEB-PAGINATION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## Generic Web pagination — evidencia observada

### Atlanticus Web

Ejecutado desde `web/`:

```text
uv run pytest framework/core/tests
53 PASS

uv run ruff check framework/core
PASS
```

### ADA Configuration

Ejecutado desde `scopes/ada/web/configuration/core`:

```text
uv run pytest
4 PASS

uv run ruff check .
PASS
```

### KPI Configuration

Ejecutado desde `scopes/ada/web/kpis/configuration`:

```text
uv run pytest
37 PASS
```

Ruff completo mostró inicialmente dos I001:

```text
src/.../web/presentation.py
→ modificado por el cutover

tests/test_web_runtime.py
→ no modificado por el cutover
```

Se corrigió exclusivamente el archivo modificado y su espejo comentado.

Validación final dirigida:

```text
ruff check callbacks.py presentation.py query.py tests/test_web_query.py
PASS

pytest
37 PASS
```

### KPI Definition

Ejecutado desde `scopes/ada/web/kpis/definition`:

```text
uv run pytest
35 PASS
```

Ruff completo mostró inicialmente cuatro I001:

```text
callbacks.py
presentation.py
query.py
→ modificados por el cutover

tests/test_web_runtime.py
→ no modificado por el cutover
```

Se corrigieron exclusivamente los tres archivos modificados y sus espejos comentados.

Validación final dirigida:

```text
ruff check callbacks.py presentation.py query.py tests/test_web_query.py
PASS

pytest
35 PASS
```

### Total observado

```text
53 + 4 + 37 + 35 = 129 tests PASS
```

Desde la raíz:

```text
git diff --check
PASS
```

El commit publicado `fbef06a8...` fue verificado en `main` después de la qualification.

## Propiedades demostradas del contrato

Los tests de `atlanticus.web.pagination` demuestran:

```text
default page size = 10
allowed page sizes = 10 | 20
other page sizes rejected
range/page-count/previous-next correct on final page
empty result resolves to page 1 and preserves page size
```

Los tests de presentación ADA demuestran interacción funcional:

```text
page metadata
previous / next enabled state
page size options 10 / 20
```

No se conservan asserts de estilo/clases CSS como contrato.

## Manager authorization — evidencia previa preservada

Permanece la evidencia del checkpoint anterior sobre:

```text
ManagerAuthorizationPolicy.can_view
explicit access_keys
PreventUpdate cardinality fix
ADA Configuration Manager smoke
```

No reinterpretar esa evidencia como qualification del consumer standalone
`navigation-manager`, que sigue en conflicto `can_access`/`can_view`.

## Política de tests Web

Probar:

```text
behavior
contracts
invariants
regressions
critical flows
```

No crear tests cuyo único objetivo sea:

```text
CSS visual
responsive
spacing
branding
apariencia
estructura visual
existencia/no existencia de funciones o clases
source token presence/absence
import presence/absence
AST/module structure
```

Los checks AST de mirrors son verificación de entrega donde ya existen, no motivo para
crear nuevos tests de estructura interna.

## Python metadata

Baseline:

```text
Python 3.14.7
```

Los comandos observados durante el cutover ejecutaron Python 3.14.7.

Permanece metadata `==3.14.2` en packages CURRENT.

```text
PYTHON-METADATA-ALIGNMENT
PLANNED / OPEN
```

## UNVERIFIED

```text
CI remoto de fbef06a8...
full Ruff workspace de fbef06a8...
full package Ruff de KPI Configuration después del fix fuera de los archivos dirigidos
full package Ruff de KPI Definition después del fix fuera de los archivos dirigidos
python:3.14.7-slim-trixie global qualification
```

Los findings I001 preexistentes de `tests/test_web_runtime.py` están fuera del alcance de
`GENERIC-WEB-PAGINATION-CUTOVER`.

Git continúa SOLO LECTURA para el asistente.
