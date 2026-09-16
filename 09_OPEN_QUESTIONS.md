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

Tools sigue siendo ADA-specific bajo `scopes/ada/web/tools`.

## CLOSED — KPI Configuration Source/Projection

```text
KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Ya no están OPEN dentro de KPI Configuration:

```text
private Source/Projection lifecycle
private source revision identity
private projection revision identity
tool_projection_revision as dependency identity
expected_source_revision
revision -> ProjectionTarget reconstruction
compatibility adapters/shims/aliases
```

KPI Configuration sigue siendo ADA-specific bajo:

```text
scopes/ada/web/kpis/configuration
```

Su dependencia exacta en Tool Projection usa `ProjectionTarget.dependencies`.

## OPEN — KPI Definition Source/Projection

```text
KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
PLANNED / NEXT
```

Preguntas permitidas en el siguiente chat:

1. ¿Qué contratos Source/Projection privados existen realmente en `scopes/ada/web/kpis/definition`?
2. ¿Qué archivos implementan authoring, Source, Projection y validación?
3. ¿Cómo consume hoy KPI Configuration Projection?
4. ¿Qué revision strings privadas existen y qué semántica representan?
5. ¿Cómo expresar la dependencia exacta usando `ProjectionTarget` sin perder semántica?
6. ¿Qué tests prueban comportamiento CURRENT y cuáles existen sólo para legacy?
7. ¿Qué consumer queda temporalmente desalineado después del cutover raíz?

No tocar Manager durante este incremento.

## OPEN — concrete KPI destination provider composition

```text
UNVERIFIED
```

El contrato `KpiDestinationCatalogProvider` está definido y el dominio no importa Tools directamente.

Este cierre no verificó la composition root productiva concreta que suministra `KpiDestinationCatalogSnapshot`.

No inventarla ni moverla al dominio KPI Configuration.

## OPEN — Python package metadata alignment

Canonical fija:

```text
Python 3.14.7
```

KPI Configuration publicado declara:

```text
requires-python = "==3.14.2"
```

Estado:

```text
PLANNED / UNVERIFIED
```

No mezclar esta limpieza con KPI Definition salvo bloqueo real de qualification.

## OPEN — Tools scoped qualification

```text
PLANNED / UNVERIFIED
```

Permanece sin evidencia nueva dentro de este cierre.

## BLOCKED — ADA Configuration Manager final cutover

```text
ADA-CONFIGURATION-MANAGER-FINAL-CUTOVER
BLOCKED
```

Bloqueado ahora por:

```text
KPI Definition Source/Projection not migrated
```

KPI Configuration ya no es blocker.

## BLOCKED — Global regression

```text
MANAGER-CONSUMER-GLOBAL-QUALIFICATION
BLOCKED
```

Ejecutar después del cutover final del Configuration Manager.

## UNVERIFIED

- full ADA suite;
- Docker E2E;
- CI remoto del checkpoint KPI Configuration;
- Python 3.14.7/Trixie global;
- provider físico/composition final de KPI destination snapshots;
- necesidad real de migración operacional de datos KPI históricos;
- KPI Definition CURRENT hasta inspección específica.

## Siguiente foco

```text
KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
```

Fuentes obligatorias:

```text
moragaga/atlanticus:main
moragaga/atlanticus-cannonical:main
```

Git sólo lectura para el asistente.
