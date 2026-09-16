# Atlanticus — Validation Baseline

Estado: **CURRENT**

## Regla

Qualification y tests son evidencia de propiedades del contrato CURRENT.

No son autoridad para conservar contratos, schemas o adapters SUPERSEDED.

No inventar un PASS cuando no existe resultado de ejecución observado.

## Autoridad de implementación

```text
moragaga/atlanticus@ee9a0401c7947f2bf61abc0a783dfa905443b6b1
```

Parent:

```text
ef3f0a44c5dcc14f8fcafe5bb36bb97865381924
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

ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## Evidencia anterior conservada

La evidencia de checkpoints previos sigue atribuida únicamente a esos checkpoints.

No trasladar resultados de Users/KPI Configuration/KPI Definition al checkpoint actual.

## Configuration Manager — evidencia observada

Antes de cerrar el cutover se observó:

```text
git diff --check
PASS

legacy token scan scoped over src/commented/tests
0 matches

python3 -m compileall scoped
PASS
```

Después, el usuario levantó el runtime local y confirmó:

```text
Configuration Manager page boot
PASS / manual smoke
```

El checkpoint publicado que contiene ese cutover es:

```text
ee9a0401c7947f2bf61abc0a783dfa905443b6b1
```

## Verificación por inspección de main

El checkpoint contiene:

```text
ConfigurationManagerDependencies con contracts CURRENT
ManagerModule generic service wiring
ManagerWorkspaceBridge
generic Source/Validation workflows
ToolConfigurationKpiDestinationCatalogProvider
local_runtime.py
__main__.py
KPI Definition dependency 0.6.0
```

El package ya no contiene `kpi_authority.py`.

Búsquedas posteriores en `main` no encontraron `expected_source_revision`, `KpiDefinitionAuthorityProvider` ni `ExactProjectionWorkflow`.

## Lo que NO está validado todavía

No existe evidencia observada en este cierre para declarar PASS de:

```text
uv lock del checkpoint final
uv sync del checkpoint final
full Ruff del Configuration Manager
full pytest del Configuration Manager
full ADA regression
edit → workspace → validate → publish → project E2E
history/reload E2E
Storage/Cosmos Docker E2E
CI remoto
```

El hecho de que la página levante no sustituye esas pruebas.

## UI

La UI volvió a estar disponible.

El usuario observó faltantes/problemas de UI y posibles contratos extraños.

Estado:

```text
ADA-CONFIGURATION-MANAGER-UI-CLEANUP
PLANNED / NEXT
```

La validation visual debe comprobar apariencia y comportamiento visible sin crear tests que congelen CSS arbitrario.

## E2E planificado

Después de UI cleanup:

```text
ADA-CONFIGURATION-MANAGER-LOCAL-E2E
PLANNED
```

Objetivo futuro:

```text
edit
→ workspace
→ validate
→ verify Source
→ publish
→ project
→ reload/status/history
```

Después:

```text
ADA-CONFIGURATION-MANAGER-STORAGE-COSMOS-E2E
PLANNED
```

Ese incremento deberá validar la topología acordada con Storage/Cosmos localizados en Docker. No se considera implementado ni diseñado por este cierre.

## Conflicto de metadata Python

Canonical fija:

```text
Python 3.14.7
```

Configuration Manager CURRENT declara:

```text
requires-python = "==3.14.2"
```

Permanece OPEN.

## Tests Web

Política CURRENT:

```text
tests protect behavior/contracts
not CSS visuals
not existence/non-existence of internal functions/classes
not accidental module structure
```

`WEB-TEST-CONTRACT-CLEANUP` continúa PLANNED y separado.

## Git

Git continúa SOLO LECTURA para el asistente.
