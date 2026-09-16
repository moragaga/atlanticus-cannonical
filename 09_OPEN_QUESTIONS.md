# Atlanticus — Open Questions

Estado: **CANONICAL OPEN ITEMS**

Los puntos aquí no reabren contratos ya CLOSED.

## CLOSED — Manager generic core

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

No están OPEN:

- dual routing exact/legacy;
- `workflow_service` como lifecycle Manager;
- `ExactSourceReaderWorkflow`;
- `ExactSourcePublicationWorkflow`;
- `ExactSourceHistoryWorkflow`;
- `ExactProjectionWorkflow` como frontera Manager;
- `expected_source_revision`;
- reconstruction revision→`ProjectionTarget`;
- `resolve_exact_source_lifecycle`;
- archivos `exact_*` dentro de Manager;
- schema workspace anterior al cutover.

## CLOSED — Navigation

```text
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

No están OPEN para Navigation:

- ubicación de Source local/Azure;
- ubicación de Projection local/Cosmos;
- adapter legacy;
- coexistencia de contratos;
- source revision browser ejecutable;
- reconstruction revision→target;
- `expected_source_revision`.

Navigation no debe reabrirse para resolver Users.

## OPEN — Users Manager alignment

```text
USERS-MANAGER-ALIGNMENT-VALIDATION
PLANNED / NEXT
```

VERIFIED mismatch:

- `users-manager` conserva `exact_history.py`;
- conserva `exact_source.py`;
- conserva `exact_projection.py`;
- `workspace.py` importa/usa `ExactSourceReadResult`;
- esos contratos `Exact*` ya no existen en Manager CURRENT;
- la full Web suite queda bloqueada durante collection.

OPEN:

1. verificar todos los imports/exports afectados;
2. revisar composición y tests de Users;
3. contrastar contra `atlanticus-cannonical:main`;
4. determinar si la intención canónica de Users ya está documentada;
5. definir el contrato final sólo con evidencia;
6. decidir después el alcance de implementación.

No asumir que la solución es simplemente borrar `exact_*`.

## OPEN — Tools consumer

```text
TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED
```

No analizar junto con Users.

## OPEN — KPI Configuration consumer

```text
KPI-CONFIG-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED
```

## OPEN — KPI Definition consumer

```text
KPI-DEFINITION-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED
```

## BLOCKED — qualification global

```text
MANAGER-CONSUMER-GLOBAL-QUALIFICATION
BLOCKED
```

Motivo inmediato:

- Navigation scoped está GREEN;
- full Web se detiene en collection por `users-manager`;
- causalidad de Navigation descartada;
- solución Users todavía no adjudicada.

## UNVERIFIED

- full Web GREEN en `d34cda3...`;
- full ADA suite;
- Docker E2E;
- CI remoto;
- Python 3.14.7/Trixie global;
- impacto de Tools/KPI hasta inspección;
- auditoría exhaustiva de `atlanticus-decisions`.

## Siguiente foco

```text
USERS-MANAGER-ALIGNMENT-VALIDATION
```

Fuentes obligatorias:

```text
moragaga/atlanticus:main
moragaga/atlanticus-cannonical:main
```
