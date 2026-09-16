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

## CLOSED — KPI Configuration Source/Projection

```text
KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## CLOSED — KPI Definition Source/Projection

```text
KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Ya no están OPEN dentro de KPI Definition:

```text
KpiDefinitionAuthority bridge
private Source/Projection lifecycle
private source revision identity
private projection revision identity
kpi_configuration_revision as dependency identity
expected_source_revision
revision -> ProjectionTarget reconstruction
compatibility adapters/shims/aliases
```

KPI Definition consume directamente la proyección tipada de KPI Configuration y su dependencia exacta usa `ProjectionTarget.dependencies`.

## OPEN — ADA Configuration Manager final generic cutover

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
PLANNED / NEXT
```

Motivo:

Todos los dominios Configuration requeridos ya están migrados, pero el consumer publicado todavía referencia contratos SUPERSEDED.

Verificado en `main@ef3f0a44...`:

```text
KpiConfigurationServices
KpiDefinitionServices
KpiDefinitionAuthorityProvider
ToolLifecycleServices
ExactProjectionWorkflow
NavigationConfigurationServices
workflow_service
exact_source_*
expected_source_revision
revision-string projection workflow adapters
```

Preguntas permitidas en el siguiente chat:

1. ¿Cuál es la composición final exacta de services por módulo usando `ManagerModule` CURRENT?
2. ¿Qué responsabilidades legítimas deben conservar `dependencies.py`, `composition.py`, `tools.py`, `kpis.py` y `kpi_definitions.py`?
3. ¿Debe `workflows.py` desaparecer por completo o conservar únicamente workflows de composición que implementen contratos genéricos reales?
4. ¿Qué uso de `kpi_authority.py` queda después de consumir directamente KPI Configuration Projection?
5. ¿Qué tests prueban comportamiento final y cuáles sólo congelan adapters/contratos SUPERSEDED?
6. ¿Qué dependencias/versiones del `pyproject.toml` deben alinearse con los packages CURRENT?
7. ¿Existen consumers externos de `ada-configuration-manager` que deban ajustarse en el mismo incremento?

No inventar las respuestas. Resolverlas contra código CURRENT antes de editar.

## OPEN — concrete KPI destination provider composition

```text
UNVERIFIED
```

El contrato de destino KPI existe, pero este cierre no adjudicó si la composición física actual del Configuration Manager es ya la definitiva o debe cambiar durante el cutover final.

No mover esta responsabilidad al dominio KPI Configuration sin evidencia.

## OPEN — Python package metadata alignment

Canonical fija:

```text
Python 3.14.7
```

Implementación publicada declara:

```text
KPI Configuration requires-python ==3.14.2
KPI Definition    requires-python ==3.14.2
```

Estado:

```text
PLANNED / UNVERIFIED
```

No mezclar esta limpieza con el Configuration Manager final salvo bloqueo real de qualification.

## OPEN — Tools scoped qualification

```text
PLANNED / UNVERIFIED
```

Permanece sin evidencia nueva dentro de este cierre.

## BLOCKED — Global regression

```text
MANAGER-CONSUMER-GLOBAL-QUALIFICATION
BLOCKED
```

Bloqueado hasta completar el cutover final de `ada-configuration-manager`.

## PLANNED — Web test contract cleanup

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / AFTER MANAGER
```

Motivo:

Existe intención explícita de revisar tests Web de existencia de funciones/clases, source-code string inspection, estructura interna y CSS visual. La política canónica ya prohíbe usar esos detalles como contrato general.

No abrir este frente durante el cutover final salvo tests legacy directamente afectados por código removido.

## UNVERIFIED

- final runtime del Configuration Manager genérico;
- full ADA suite después de ese cutover;
- Docker E2E;
- CI remoto del checkpoint `ef3f0a44...`;
- Python 3.14.7/Trixie global;
- provider físico/composition final de KPI destination snapshots;
- necesidad real de migración operacional de datos KPI históricos.

## Siguiente foco

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
```

Fuentes obligatorias:

```text
moragaga/atlanticus:main
moragaga/atlanticus-cannonical:main
```

Git sólo lectura para el asistente.
