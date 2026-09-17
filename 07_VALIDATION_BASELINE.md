# Atlanticus — Validation Baseline

Estado: **CURRENT**

## Regla

Qualification y tests son evidencia de propiedades del contrato CURRENT.

No son autoridad para conservar contratos, schemas o adapters SUPERSEDED.

No inventar un PASS cuando no existe resultado de ejecución observado.

## Autoridad de implementación

```text
moragaga/atlanticus@4e008055ddc551e6c08a7d87715340c8c7cd149e
```

Parent:

```text
709cf2fb9ee422094f011cfda051f08f37276992
```

## Hitos contractuales relevantes

```text
USERS-STANDALONE-AUTHORITY-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-BOUNDARY-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CAPABILITY-EXTRACTION
IN PROGRESS
```

Los hitos genéricos de Manager, Navigation, Users legacy removal, Tools, KPI
Configuration, KPI Definition y ADA Configuration Manager previamente cerrados
permanecen CURRENT.

## Users standalone — evidencia observada

Antes de publicar `709cf2f...`:

```text
users/core                 41 PASS
users/cosmos               22 PASS
users/projection-cosmos    29 PASS
TOTAL                      92 PASS

git diff --check
PASS
```

Esto demuestra comportamiento afectado por el authority/runtime cutover.

No demuestra wiring completo del selector local en todas las compositions.

## Profiles configuration boundary — evidencia observada

Antes de publicar `4e008055...`:

```text
profiles/core              7 PASS
profiles/configuration     2 PASS
users/configuration       65 PASS
TOTAL                     74 PASS

uv lock
PASS

git diff --check
PASS
```

El checkpoint publicado contiene el package
`atlanticus-web-profiles-configuration==0.1.0` y el movimiento de
`ProfilesConfiguration` fuera de core.

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

## Conflicto CURRENT de test hygiene

Existe en CURRENT:

```text
web/capabilities/users/core/tests/test_authority.py
test_users_core_has_no_profiles_dependency
```

Ese test lee `pyproject.toml` y los `.py` de `src` para comprobar ausencia de
Profiles.

Clasificación:

```text
VERIFIED
POLICY CONFLICT
OPEN
```

No bloquea el estado funcional ya publicado, pero no debe replicarse ni usarse
como patrón.

Su cleanup pertenece a:

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED
```

No mezclar con `USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER`.

## Lo que NO está validado todavía

No existe evidencia en este cierre para declarar PASS de:

```text
full web workspace pytest
full Ruff workspace
full ADA regression
CI remoto
productivo local Jane/John wiring
Users + Profiles final composition
Profiles independent Source/Projection lifecycle
Profiles UI
Navigation => Profiles => Users composition
Python 3.14.7 metadata alignment
```

## UI / CSS

No integrar validaciones CSS visuales en suites contractuales.

Apariencia, responsive, spacing y branding se validan visualmente.

No convertir markup/CSS incidental en contrato automatizado.

## Conflicto de metadata Python

Canonical fija:

```text
Python 3.14.7
```

Packages CURRENT todavía contienen metadata:

```text
requires-python = "==3.14.2"
```

Permanece OPEN.

## Git

Git continúa SOLO LECTURA para el asistente.
