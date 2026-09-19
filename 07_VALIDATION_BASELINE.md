# Atlanticus — Validation Baseline

Estado: **CURRENT**

## Regla

Qualification y tests son evidencia de propiedades del contrato CURRENT.

No son autoridad para conservar contratos, schemas, adapters o aliases SUPERSEDED.

No inventar un PASS cuando no existe resultado observado.

## Autoridad de implementación

```text
moragaga/atlanticus@9f12c41a23d69784c7c5b775a4093a94ac654d55
```

Parent:

```text
3eb46dac80f23d438774e3afa39999dc96f592d7
```

Tree:

```text
dd002b632b494065428af9dd10f1e58b7e6638d1
```

## Hitos contractuales relevantes

```text
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
CLOSED / VERIFIED / CURRENT

MANAGER-AUTHORIZATION-SEMANTICS-ALIGNMENT
CLOSED / VERIFIED / CURRENT

MANAGER-ACTIVE-WORKFLOW-CALLBACK-CARDINALITY
CLOSED / VERIFIED / CURRENT
```

## Manager authorization — evidencia observada

Durante el incremento se observó:

```text
rg stale Manager authorization symbols
0 matches en el scope ejecutado

uv lock --check [web]
PASS

pytest capabilities/manager/tests compositions/navigation-manager/tests
68 PASS

ruff check focused Manager + navigation-manager files
PASS

ruff format --check focused Manager + navigation-manager files
PASS

full web pytest
PASS / 100%
7 skipped
```

La suite full web anterior fue ejecutada antes del último delta de callback. Por tanto no
atribuir ese full PASS específicamente al estado posterior al `PreventUpdate` sin rerun.

## ADA Configuration Manager — evidencia observada

Después de alinear:

```text
atlanticus-web-navigation-configuration[web]==0.1.9
```

se observó:

```text
uv lock --check
PASS

focused Ruff
PASS

focused format --check
PASS

pytest
26 PASS
```

Ese full ADA pytest también precede al último delta del callback genérico.

## Callback cardinality regression

Problema observado antes del fix:

```text
InvalidCallbackReturnValue
Expected 1, got 0
```

sobre outputs pattern `ALL` de `refresh_active_workflow`.

Fix CURRENT:

```text
route/module no resoluble o no visible
→ raise PreventUpdate
```

Después del fix se observó:

```text
productive/commented callbacks AST-equivalent
PASS

targeted callback regression tests
PASS

git diff --check
PASS dentro del repair script

manual smoke /manager
HTTP 200/204
sin 500 observado
sin InvalidCallbackReturnValue observado
```

## Conflict detectado durante cierre documental

Inspección de `main@9f12c41...` demuestra:

```text
ManagerAuthorizationPolicy
→ can_view(...)
```

pero:

```text
web/compositions/navigation-manager
→ resolved_authorization.can_access(...)
```

Estado:

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

Los tests ejecutados no demostraron el callback `can_manage` de ese consumer standalone.
No considerar el consumer funcional por inferencia.

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

El check AST usado durante reparación del espejo fue una verificación de entrega puntual,
no un contrato de producto que deba proliferar como test permanente.

## Python metadata

Baseline:

```text
Python 3.14.7
```

El entorno observado ejecutó Python 3.14.7.

Permanece metadata `==3.14.2` en packages CURRENT.

```text
PYTHON-METADATA-ALIGNMENT
PLANNED / OPEN
```

## UNVERIFIED

```text
full web pytest después del último callback delta
full ADA pytest después del último callback delta
full Ruff workspace
CI remoto de 9f12c41...
python:3.14.7-slim-trixie global qualification
```

Git continúa SOLO LECTURA para el asistente.
