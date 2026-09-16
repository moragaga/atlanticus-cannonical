# Atlanticus — Open Questions

Estado: **CANONICAL OPEN ITEMS**

Los puntos aquí no reabren contratos CLOSED.

## CLOSED — Manager generic core

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

No están OPEN:

```text
double routing exact/legacy
workflow_service lifecycle
ExactProjectionWorkflow
expected_source_revision
revision -> ProjectionTarget reconstruction
compatibility shims/adapters
```

## CLOSED — Configuration domains

```text
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-MANAGER-GENERIC-CONTRACT-CUTOVER
CLOSED / VERIFIED / CURRENT

TOOLS-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## CLOSED — ADA Configuration Manager final generic cutover

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Ya no están OPEN en el consumer:

```text
KpiConfigurationServices
KpiDefinitionServices
KpiDefinitionAuthorityProvider
ToolLifecycleServices
NavigationConfigurationServices
ExactProjectionWorkflow
workflow_service
exact_source_*
expected_source_revision
revision-string projection adapters
```

## OPEN — Configuration Manager UI cleanup

```text
ADA-CONFIGURATION-MANAGER-UI-CLEANUP
PLANNED / NEXT
```

Motivo:

La página ya levanta, pero el usuario observó faltantes/problemas de UI.

También indicó que algunos contratos “quedaron raros”, sin adjudicar todavía cuáles ni si requieren cambio.

Preguntas permitidas en el siguiente chat:

1. ¿Qué problemas de UI son reproducibles en la aplicación CURRENT?
2. ¿Qué archivos son realmente responsables de cada problema?
3. ¿Existe un contrato funcional incorrecto detrás de alguno de esos síntomas?
4. ¿Puede corregirse cada problema sin reabrir contratos congelados?
5. ¿Qué validación visual/manual y qué tests funcionales corresponden a cada corrección?

No inventar respuestas ni modificar contratos sólo por apariencia.

## OPEN — local behavioral E2E

```text
ADA-CONFIGURATION-MANAGER-LOCAL-E2E
PLANNED / AFTER UI CLEANUP
```

Todavía no se verificó de punta a punta:

```text
edit
workspace
validate
source verification
publish
project
reload/status
history
```

## OPEN — Storage/Cosmos Docker E2E

```text
ADA-CONFIGURATION-MANAGER-STORAGE-COSMOS-E2E
PLANNED / AFTER LOCAL E2E
```

No está diseñado ni ejecutado por este cierre.

La topología concreta deberá derivarse de los contratos CURRENT y de los providers existentes, no inventarse.

## OPEN — Manager consumer global qualification

```text
MANAGER-CONSUMER-GLOBAL-QUALIFICATION
PLANNED / UNBLOCKED
```

El cutover ya no lo bloquea.

Permanece sin evidencia observada de full Ruff/pytest/full ADA regression para el checkpoint `ee9a0401...`.

## OPEN — Python package metadata alignment

Canonical fija:

```text
Python 3.14.7
```

Configuration Manager CURRENT declara:

```text
requires-python = "==3.14.2"
```

Estado:

```text
PLANNED / UNVERIFIED
```

No mezclar con UI cleanup salvo bloqueo real.

## PLANNED — Web test contract cleanup

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED
```

La política vigente ya prohíbe congelar CSS visual o implementación interna como contrato general.

No abrir este frente durante UI cleanup salvo un test directamente afectado por un cambio funcional legítimo.

## UNVERIFIED

- detalle exacto de todos los problemas UI;
- naturaleza exacta de los contratos percibidos como raros;
- full Ruff/pytest del Configuration Manager en `ee9a0401...`;
- full ADA regression;
- local behavioral E2E;
- Storage/Cosmos Docker E2E;
- CI remoto;
- Python 3.14.7/Trixie global.

## Siguiente foco

```text
ADA-CONFIGURATION-MANAGER-UI-CLEANUP
```

Fuentes obligatorias:

```text
moragaga/atlanticus:main
moragaga/atlanticus-cannonical:main
```

Git sólo lectura para el asistente.
