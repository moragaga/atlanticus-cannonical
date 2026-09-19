# Atlanticus — Validation Baseline

Estado: **CURRENT**

## Regla

Qualification y tests son evidencia de propiedades del contrato CURRENT.

No son autoridad para conservar contratos, schemas o adapters SUPERSEDED.

No inventar un PASS cuando no existe resultado de ejecución observado.

## Autoridad de implementación

```text
moragaga/atlanticus@3eb46dac80f23d438774e3afa39999dc96f592d7
```

Parent:

```text
0fba548329afd9bc9dee92ea6caa53d1aaa69eb0
```

Tree:

```text
69386cf40e566baad2786a079029a6eea20bd8d1
```

## Hitos contractuales relevantes

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-BOUNDARY-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CAPABILITY-EXTRACTION
CLOSED / VERIFIED / CURRENT

PROFILES-INDEPENDENT-SOURCE-LIFECYCLE
CLOSED / VERIFIED / CURRENT

USERS-PERSISTED-DATA-CUTOVER
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROFILES-CONFIGURATION
CLOSED / VERIFIED / CURRENT

NONPROMOTED-ACCESS-SEMANTICS-CORRECTION
CLOSED / VERIFIED / CURRENT

NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
CLOSED / VERIFIED / CURRENT
```

Los hitos genéricos de Manager, Navigation, Tools, KPI Configuration, KPI Definition
y ADA Configuration Manager previamente cerrados permanecen CURRENT.

## Navigation / Profiles alignment — evidencia observada

Entorno reportado por el usuario:

```text
Fedora
Python 3.14.7
uv
```

### Lock

```text
uv lock --check
Resolved 77 packages
PASS
```

### Tests focalizados

```text
uv run --locked pytest \
  capabilities/navigation/configuration/tests \
  compositions/navigation-manager/tests \
  -q
```

Resultado:

```text
PASS / 100%
```

### Regresión Profiles + Navigation

```text
uv run --locked pytest \
  capabilities/profiles/core/tests \
  capabilities/navigation/core/tests \
  capabilities/navigation/configuration/tests \
  compositions/navigation-manager/tests \
  -q
```

Resultado:

```text
PASS / 100%
```

### Ruff sobre archivos modificados del incremento

Después de aplicar formatter únicamente a los cuatro archivos Navigation Manager
modificados por este incremento:

```text
uv run --locked ruff check \
  compositions/navigation-manager/src/atlanticus/web/compositions/navigation_manager/composition.py \
  compositions/navigation-manager/src/atlanticus/web/compositions/navigation_manager/workflows.py \
  compositions/navigation-manager/tests/test_composition.py \
  compositions/navigation-manager/tests/test_workflows.py
```

Resultado:

```text
All checks passed!
```

Y:

```text
uv run --locked ruff format --check \
  compositions/navigation-manager/src/atlanticus/web/compositions/navigation_manager/composition.py \
  compositions/navigation-manager/src/atlanticus/web/compositions/navigation_manager/workflows.py \
  compositions/navigation-manager/tests/test_composition.py \
  compositions/navigation-manager/tests/test_workflows.py
```

Resultado:

```text
4 files already formatted
```

### Git diff check

```text
git diff --check
PASS
```

### Legacy symbol scan

Búsqueda ejecutada:

```text
NavigationProfileOption
NavigationProfileOptionsProvider
profile_options_provider
_BASE_PROFILES
resolve_profile_options
selectable_profile_options
profile--unrestricted
projection_validators
```

Scope:

```text
capabilities/navigation/configuration
compositions/navigation-manager
```

Resultado:

```text
0 matches
```

## Ruff global / cleanup transversal

Antes de limitar Ruff al ownership real del incremento se observaron findings fuera del
scope inmediato:

```text
capabilities/navigation/configuration/tests/test_web_contract.py
capabilities/navigation/configuration/tests/test_web_source_contract.py
```

El primer archivo produjo un `I001`; ambos aparecieron en `ruff format --check` del
directorio completo.

No fueron modificados oportunistamente.

Estado:

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN

full Ruff workspace
UNVERIFIED / NOT CLAIMED PASS
```

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
detalles internos
```

CSS/branding/responsive/spacing se validan visualmente salvo comportamiento
funcional automatizable.

Assets JS/CSS sólo se automatizan por existencia/carga cuando esa carga sea parte
real del contrato.

## CI remoto

No se verificaron status checks/workflow runs asociados a `3eb46dac...` durante este
cierre.

Estado:

```text
UNVERIFIED
```

## Python metadata

Canonical fija:

```text
Python 3.14.7
```

La qualification local de este incremento se ejecutó bajo 3.14.7.

Navigation Configuration CURRENT todavía contiene:

```text
requires-python = "==3.14.2"
```

Estado:

```text
VERIFIED CONFLICT / OPEN
```

## Git

Git continúa SOLO LECTURA para el asistente.
