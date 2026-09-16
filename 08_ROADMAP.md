# Atlanticus — Roadmap

Estado: **CANONICAL BASELINE 1.0 — EXECUTION IN PROGRESS**

## Regla

Un solo foco por incremento.

Cerrar cada frontera con evidencia suficiente para su alcance.

No conservar legacy para sostener consumers o tests anteriores.

## Checkpoint publicado de referencia

```text
moragaga/atlanticus@ee9a0401c7947f2bf61abc0a783dfa905443b6b1
```

Parent:

```text
ef3f0a44c5dcc14f8fcafe5bb36bb97865381924
```

## Hitos cerrados

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

## Configuration Manager closure

El consumer final está publicado y la página local vuelve a levantar.

El cierre de este hito cubre:

```text
generic Manager contract adoption
workspace bridge adoption
legacy consumer removal
local smoke runtime
static validation
manual UI boot
```

No cubre full E2E ni UI completeness.

## Siguiente foco único

```text
ADA-CONFIGURATION-MANAGER-UI-CLEANUP
PLANNED / NEXT
```

Reglas del siguiente chat:

1. trabajar sólo sobre problemas visibles/reproducibles de la UI del Configuration Manager CURRENT;
2. inspeccionar código CURRENT antes de proponer cambios;
3. no reabrir Manager core, Source core, Projection core ni dominios ya cerrados sin conflicto demostrado;
4. si un contrato parece extraño, verificar su uso y responsabilidad antes de cambiarlo;
5. no introducir legacy, aliases, shims ni doble contrato;
6. no mezclar E2E en este incremento;
7. tests sólo para comportamiento automatizable; apariencia se valida visualmente;
8. cambios pequeños y verificables.

## Después del UI cleanup

```text
MANAGER-CONSUMER-GLOBAL-QUALIFICATION
PLANNED / UNBLOCKED
```

y, como incremento separado:

```text
ADA-CONFIGURATION-MANAGER-LOCAL-E2E
PLANNED
```

El E2E local deberá validar el flujo real del Manager sin añadir todavía Storage/Cosmos Docker.

## Después del local E2E

```text
ADA-CONFIGURATION-MANAGER-STORAGE-COSMOS-E2E
PLANNED
```

Su objetivo será validar la misma semántica con infraestructura local de Storage/Cosmos, sólo después de que el flujo local esté estable.

## Open independiente

```text
Python 3.14.7 metadata alignment
PLANNED / OPEN

WEB-TEST-CONTRACT-CLEANUP
PLANNED

CI remote
UNVERIFIED
```

Configuration Manager todavía declara:

```text
requires-python = "==3.14.2"
```

No resolver este punto dentro de UI cleanup salvo bloqueo directo.

## No mezclar en el siguiente chat

- local E2E;
- Storage/Cosmos Docker E2E;
- Python baseline cleanup;
- cleanup transversal de tests Web;
- Command Center;
- Operational Data;
- rediseño de Manager core;
- rediseño de Source/Projection core;
- reintroducción de contratos legacy.

Único foco:

```text
ADA-CONFIGURATION-MANAGER-UI-CLEANUP
```
