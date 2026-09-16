# Atlanticus — Open Questions

Estado: **CANONICAL OPEN ITEMS**

Los puntos aquí no reabren contratos CLOSED.

## CLOSED — Manager generic core

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

No están OPEN:

- doble routing exact/legacy;
- `workflow_service` como lifecycle Manager;
- `ExactSource*` como frontera Manager;
- `ExactProjectionWorkflow` como frontera Manager;
- `expected_source_revision`;
- reconstruction revision→`ProjectionTarget`;
- shims/adapters de compatibilidad Manager.

## CLOSED — Navigation

```text
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## CLOSED — Users

```text
USERS-MANAGER-GENERIC-CONTRACT-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-CLEAN-CUTOVER-COMPLETION
CLOSED / VERIFIED / CURRENT

USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL
CLOSED / VERIFIED / CURRENT
```

## CLOSED — Tools Source/Projection

```text
TOOLS-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Ya no están OPEN dentro del dominio Tools:

```text
ToolLifecycleServices
ToolConfigurationSourceSnapshot
ToolConfigurationProjectionSnapshot
ToolConfigurationProjectionRepository
ToolConfigurationPublisher
ToolConfigurationSource
expected_source_revision
private projection revision identity
```

Tools sigue siendo ADA-specific bajo `scopes/ada/web/tools`.

## OPEN — Tools scoped qualification

```text
PLANNED / UNVERIFIED
```

Los tests CURRENT existen, pero este cierre no aporta resultado de ejecución.

## OPEN — KPI Configuration Source/Projection

```text
KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
PLANNED / NEXT
```

Preguntas permitidas en el siguiente chat:

1. ¿Qué contratos Source/Projection privados siguen existiendo exactamente en `scopes/ada/web/kpis/configuration`?
2. ¿Qué archivos implementan authoring, Source, Projection y validación?
3. ¿Cómo se obtiene hoy la autoridad/destination catalog desde Tool Projection?
4. ¿Dónde se usa `tool_projection_revision` y qué invariantes representa realmente?
5. ¿Cómo expresar esa dependencia usando `ProjectionTarget`/dependencies sin perder semántica?
6. ¿Qué tests legacy deben eliminarse y qué comportamiento CURRENT debe preservarse?

No tocar Manager durante este incremento.

## OPEN — KPI Definition Source/Projection

```text
KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
PLANNED
```

No analizar junto con KPI Configuration salvo para identificar una frontera contractual que KPI Configuration deba exponer.

## OPEN — Tools Manager consumer

```text
TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED
```

`ada-configuration-manager` todavía importa `ToolLifecycleServices` y define `ToolConfigurationManagerWorkflowAdapter`.

No corregirlo con compatibilidad dentro de Tools.

## BLOCKED — ADA Configuration Manager final cutover

```text
ADA-CONFIGURATION-MANAGER-FINAL-CUTOVER
BLOCKED
```

Bloqueado por:

```text
KPI Configuration Source/Projection not migrated
KPI Definition Source/Projection not migrated
```

## BLOCKED — Global regression

```text
MANAGER-CONSUMER-GLOBAL-QUALIFICATION
BLOCKED
```

Ejecutar después del cutover final del Configuration Manager.

## UNVERIFIED

- Tools scoped pytest/Ruff;
- full ADA suite;
- Docker E2E;
- CI remoto;
- Python 3.14.7/Trixie global;
- provider físico final de Tool Source/Projection;
- necesidad real de migración operacional de datos Tool antiguos.

## Siguiente foco

```text
KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
```

Fuentes obligatorias:

```text
moragaga/atlanticus:main
moragaga/atlanticus-cannonical:main
```

Git sólo lectura para el asistente.
