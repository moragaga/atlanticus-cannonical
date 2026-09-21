# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Checkpoint CURRENT verificado para este cierre:
  `1c67212b21ef2241bcb59173ccb8e9cd237a0219`
- Parent inmediato:
  `07eeb8d4ecc3f1e9d9a84ab1059eaad2fd5f78ce`
- Tree:
  `ae432b5aa55183ca9f11b35ac37f4fa3859c9e78`
- Fecha del commit:
  `2026-09-21T13:25:55Z`

Estado acumulado relevante:

```text
ADA-STORAGE-NAMESPACE                         CLOSED / VERIFIED / CURRENT
TOOL-PROJECTION-PERSISTENCE                   CLOSED / VERIFIED / CURRENT
TOOL-PERSISTENCE-RESILIENT-COMPOSITION        CLOSED / VERIFIED / CURRENT

ADA-WEB-KPI-COLLECTOR-CAPABILITY              CLOSED / VERIFIED / CURRENT
ADA-GENERIC-OPERATIONAL-BOOTSTRAP             CLOSED / VERIFIED / CURRENT
ADA-GENERIC-COLLECTOR-RUNTIME-WIRING          CLOSED / VERIFIED / CURRENT
ADA-GENERIC-OPERATIONAL-RENDER-RUNTIME-CONTRACT
                                               CLOSED / VERIFIED / CURRENT
ADA-GENERIC-STAGE-1                           CLOSED / VERIFIED / CURRENT

COMMAND-CENTER-WEB-ALARM-CONFIGURATION-CONTRACT
                                               CLOSED / VERIFIED / CURRENT
COMMAND-CENTER-ALARM-CONFIGURATION-PROJECTION CLOSED / VERIFIED / CURRENT
COMMAND-CENTER-ALARM-CONFIGURATION-MANAGER-INTEGRATION
                                               CLOSED / VERIFIED / CURRENT
```

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Checkpoint inspeccionado antes de este reemplazo:
  `f04ee728b157a4f64a3c0c59622d5d6702f4cd87`

`atlanticus-cannonical:main` es autoridad documental vigente, subordinada a
`atlanticus:main` cuando la implementación publicada demuestra un estado posterior.

### Historical decisions

- Repositorio: `moragaga/atlanticus-decisions`
- Rama: `main`
- Checkpoint observado:
  `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`

Permanece **HISTORICAL**.

Las decisiones B.1/B.2 preservan semántica útil de AlarmDefinition, Live/Management y
Runtime/Delivery, pero sus bindings físicos históricos SharePoint/Cosmos no prevalecen sobre
Source/Projection/Blob CURRENT.

## Jerarquía

1. `atlanticus:main`: realidad implementada.
2. `atlanticus-cannonical:main`: contratos, fronteras, roadmap y estado vigente.
3. Qualification/tests vigentes: evidencia de propiedades demostradas.
4. Decisiones explícitas del Project todavía no formalizadas en canonical: delta temporal.
5. `atlanticus-decisions`: referencia histórica.
6. Memoria/historial conversacional: pista, nunca autoridad suficiente.

## Clasificación obligatoria

```text
VERIFIED
INFERRED
ASSUMED
PROPOSED
UNVERIFIED
```

Estados:

```text
CURRENT
IN PROGRESS
PLANNED
SUPERSEDED
BLOCKED
CLOSED
```

Si implementación y canonical se contradicen, exponer el conflicto y actualizar canonical;
nunca retroceder implementación CURRENT para satisfacer documentación obsoleta.

## Git

Git es **READ ONLY** por defecto.

No crear commits, push, ramas, PR, issues ni mutaciones remotas sin autorización explícita.

## Continuidad congelada

No reabrir sin conflicto demostrado:

```text
LEGACY
REMOVE

ADAPTERS / SHIMS / ALIASES
FORBIDDEN

DOUBLE CONTRACT
FORBIDDEN

OLD SCHEMA RUNTIME READERS
FORBIDDEN

revision -> ProjectionTarget reconstruction
REMOVE

expected_source_revision
REMOVE
```

## Storage namespace CURRENT

ADA dispone de:

```text
AdaStorageNamespace
application_namespace
tool_namespace
```

Separación congelada:

```text
physical container / connection
!=
application namespace
!=
tool namespace
!=
SourceKey
```

El root de Tool entregado al Source provider termina en:

```text
<application>/<tool>
```

`SourceStore` agrega internamente:

```text
sources/<SourceKey>
```

La composición no conoce ni duplica ese segmento.

## Tool Projection CURRENT

Persistencia durable:

```text
LocalToolProjectionStore
CosmosToolProjectionStore
```

Runtime activo:

```text
resolve_active_tool_projection()
→ durable Tool Projection
```

Source sólo participa en workflows de materialización/update:

```text
project_current_tool_source()
```

## Tool persistence composition CURRENT

Providers independientes:

```text
Source     = local | blob
Projection = local | cosmos
```

Estados de resolución:

```text
READY
UNCONFIGURED
UNAVAILABLE
INVALID
```

La composición no ejecuta health checks ni lecturas remotas obligatorias durante construcción.

## Invariante de disponibilidad

```text
APPLICATION EXISTENCE
!=
TOOL CONFIGURATION EXISTENCE
!=
EXTERNAL INFRASTRUCTURE AVAILABILITY
!=
BUSINESS DATA AVAILABILITY
```

ADA Generic aplica esta regla en su bootstrap CURRENT.

`UNCONFIGURED`, `UNAVAILABLE` e `INVALID` de Tool no eliminan la Web base.

La ausencia de KPI Delivery no define la existencia del proceso Web.

## ADA Generic Stage 1 CURRENT

Cadena implementada:

```text
environment / .env
→ provider settings
→ AdaStorageNamespace
→ ToolPersistenceComposition
→ resolve_active_tool_projection()
→ ToolStructure
→ AdaKpiCollector
→ Latest Delivery / Timeseries Delivery
→ process cache
→ browser dcc.Store por ToolComponent
→ frontera de consumo del desarrollador
```

Semántica congelada:

```text
Latest polling      = 10 s default
Timeseries polling  = 120 s default
Browser refresh     = 10 s default
Latest priority     = before Timeseries when both are due
1 ToolComponent     = 1 logical/browser KPI Store
Subcomponent        != Store
browser             = cache only
```

## Frontera de render CURRENT

`OperationalRenderBinding` es estructural.

Contiene:

```text
ToolStructure
OperationalComponentBinding -> ToolComponent
```

No contiene:

```text
ComponentStoreSnapshot
KPI payload
Collector state
browser state
```

`AdaKpiCollector` no depende de `ada-web-operational-render-binding`.

ADA Generic entrega los datos operacionales hasta los `dcc.Store` existentes.

La visualización concreta de una Tool pertenece al desarrollador/aplicación específica.

Por tanto:

```text
ADA-GENERIC-OPERATIONAL-STORE-TO-RENDER-WIRING
SUPERSEDED / NOT REQUIRED
```

No crear body genérico obligatorio, adapter de KPI a render ni segunda copia de estado.

## ADA Command Center Alarm Configuration CURRENT

Implementado bajo:

```text
scopes/ada-command-center/web/alarms/configuration
```

Cadena CURRENT:

```text
Manager Workspace
→ intrinsic validation
→ Alarm Configuration Source/Release
→ Alarm Configuration Projection
```

La unidad publicada es atómica:

```text
Alarm Rules + Message Catalog
```

La Projection base conserva la Alarm Configuration de una `SourceRelease` exacta y no introduce
Tool Catalog, evaluator resolution, B.2, Runtime ni Delivery.

La composición Manager reutiliza `atlanticus.web.manager`; no existe un Manager paralelo de
Command Center.

La superficie Web capability-local existe en modo documental. El editor visual final de Rules,
Messages y parameters permanece abierto sin cambiar el contrato durable.

## Siguiente foco único

```text
COMMAND-CENTER-TOOL-CATALOG-CONTRACT
PLANNED / NEXT / DESIGN FIRST
```

Command Center Tool Catalog es una capability distinta de ADA Tool Configuration.

ADA Tool Configuration conserva authoring/ownership de `tool_key`, kind, Components,
Subcomponents y topología.

Command Center Tool Catalog es un **consolidador/reconciliador read-only** de Tool projections
externas. No crea Tools, no edita Tools, no se convierte en segunda source of truth y no debe
inventar un fork de los contratos CURRENT de Tool Configuration/ToolStructure.

El siguiente chat debe congelar primero el contrato productor consumible por B.2. No implementar
B.2, Runtime Delivery, Cosmos reconciliation ni Blob binding antes de cerrar ese contrato.
