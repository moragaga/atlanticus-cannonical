# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Checkpoint CURRENT verificado para este cierre:
  `bc8eafc21a65e3f9aff044c232e2562cd490c49f`
- Parent inmediato:
  `d6e405e6466b1bf8d29dadae442a03062da2f1b3`
- Tree:
  `c26c0ee18161ca7fc49c439109bec99ecae77476`
- Fecha del commit:
  `2026-09-21T11:17:34Z`

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
```

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Checkpoint inspeccionado antes de este reemplazo:
  `5c29631526939c52528e147b4a83e5557e610bf0`

`atlanticus-cannonical:main` es autoridad documental vigente, subordinada a
`atlanticus:main` cuando la implementación publicada demuestra un estado posterior.

### Historical decisions

- Repositorio: `moragaga/atlanticus-decisions`
- Rama: `main`
- Checkpoint observado:
  `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`

Permanece **HISTORICAL**.

No se verificó una decisión histórica específica que contradiga el cierre de ADA Generic Stage 1.
Las búsquedas por bootstrap, operational render y Command Center Alarm Configuration no devolvieron
un contrato histórico aplicable. Cualquier contradicción histórica adicional permanece
`UNVERIFIED`.

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

## Siguiente foco único

```text
ADA-COMMAND-CENTER-ALARM-CONFIGURATION
PLANNED / NEXT
```

El siguiente chat debe auditar primero contratos e implementación CURRENT de Command Center y
Alarm Engine antes de diseñar o implementar.

Objetivo de siguiente frontera:

```text
human authoring
→ Alarm Configuration contract
→ validation
→ Source / Release
→ durable Projection / materialization
→ Alarm Engine consumption
→ alarm state delivery boundary
→ developer-owned visualization
```

No implementar esa cadena desde este cierre.
