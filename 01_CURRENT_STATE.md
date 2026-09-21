# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

## Autoridad

```text
Implementation
moragaga/atlanticus@d484569cbe0290f38f239481cde81b13a23deecf

Parent
dde1e3a114a04b22cc2118c347a7ed907852c06b

Tree
4c7c8209f2d0c670d3c6e8b5185b5af12172e591

Canonical inspected before replacement
moragaga/atlanticus-cannonical@a8c8c80ed3392cb189923d00bd5037e5965e2da5
```

Git permanece SOLO LECTURA.

## Estado resumido

```text
KPI-REGISTRY-CAPABILITY-CUTOVER                 CLOSED / VERIFIED / CURRENT
KPI-DEFINITION-CAPABILITY-CUTOVER               CLOSED / VERIFIED / CURRENT
KPI-RUNTIME-REPROCESS-CURRENT                   CLOSED / VERIFIED / CURRENT
KPI-DELIVERY-REGISTRY-CONSUMPTION               CLOSED / VERIFIED / CURRENT
KPI-TIMESERIES-REGISTRY-CONSUMPTION             CLOSED / VERIFIED / CURRENT
KPI-HISTORIAN-REPROCESS-CURRENT                 CLOSED / VERIFIED / CURRENT

ATLANTICUS-WEB-OBSERVABILITY-SERVICE            CLOSED / VERIFIED / CURRENT
ADA-WEB-KPI-COLLECTOR-CAPABILITY                CLOSED / VERIFIED / CURRENT
KPI-COLLECTOR-DEFINITION-ATTACHMENT             CLOSED / VERIFIED / CURRENT
KPI-COLLECTOR-REAL-WEB-SMOKE                    CLOSED / VERIFIED / CURRENT

ADA-GENERIC-COLLECTOR-OPERATIONAL-INTEGRATION   PLANNED / NEXT

KPI-INSPECTION-DEFINITION-PROVIDER-REALIGNMENT  OPEN / SEPARATE
PYTHON-METADATA-ALIGNMENT                       OPEN / SEPARATE
FULL-BACKEND-PYTEST-TOPOLOGY                    BLOCKED / UNVERIFIED AS PREEXISTING / SEPARATE
```

## Collector CURRENT

Package:

```text
scopes/ada/web/kpis/collector
ada-web-kpi-collector==0.1.0
```

Scheduling default:

```text
Latest      10 s
Timeseries 120 s
Browser     10 s
```

Latest y Timeseries son lecturas independientes. Cuando ambas están due, Latest se procesa
primero.

El collector valida los documentos materializados CURRENT:

```text
Latest
ada-kpi-latest-delivery / latest / kpis / schema 1

Timeseries
ada-kpi-timeseries-delivery / timeseries / kpis / schema 2
```

Compatibilidad server-side:

```text
(configuration_revision, tool_projection_revision)
```

Monotonicidad:

```text
Latest     watermark_utc no puede retroceder
Timeseries end_utc no puede retroceder
```

Un Latest nuevo compatible puede avanzar sin esperar Timeseries. Si Latest cambia
compatibilidad, Timeseries incompatible se elimina del snapshot. Un Timeseries incompatible
con Latest no desplaza el estado vigente.

Missing document conserva el último estado bueno; contract/source errors no mutan el cache.

## Component stores CURRENT

`ToolStructure.components` define los stores.

```text
1 ToolComponent = 1 logical KPI Component Store
Subcomponent    != Store
system destinations != Component Store
```

Store browser id:

```text
{
  type: ada-kpi-component-store,
  tool: <tool_key>,
  component: <component_key>
}
```

Cada store contiene, cuando existan:

```text
latest
Timeseries
```

Browser callback lee únicamente el cache del proceso. Nunca consulta Cosmos.

El merge browser evita regresión entre workers usando:

```text
Latest     configuration_revision + watermark_utc + revision
Timeseries configuration_revision + end_utc + revision
```

## Lifecycle CURRENT

```text
one poller/cache per worker
first real application request -> ensure_started()
/health/*                     -> does not start poller
/assets/*                     -> does not start poller
/.auth/*                      -> does not start poller
```

El polling corre en thread daemon por PID. Requests no hacen lectura Cosmos inline.

## Web Observability CURRENT

Atlanticus Web registra su instancia runtime como servicio:

```text
WEB_OBSERVABILITY_SERVICE_KEY
atlanticus.web.observability
```

El collector requiere ese servicio y reporta incidentes deduplicados:

```text
Delivery read failure -> WARNING
contract failure      -> ERROR
other refresh failure -> ERROR
runtime failure       -> CRITICAL
```

## Attachment CURRENT

Una definición Web ya resuelta puede decorarse mediante:

```text
attach_ada_kpi_collector(definition, collector)
```

El attachment:

- agrega el módulo `ada-kpi-collector`;
- envuelve el layout con interval/revision/component stores;
- rechaza attachment duplicado;
- no vuelve obligatoria la dependencia desde `ada-generic-application`.

## Siguiente frontera

La capability Collector está cerrada. Sigue OPEN únicamente su montaje operacional real:

```text
ADA-GENERIC-COLLECTOR-OPERATIONAL-INTEGRATION
PLANNED / NEXT
```

El próximo chat debe integrar el collector, no rediseñarlo.

Debe localizar la composición CURRENT que posee:

```text
ToolConfiguration / ToolStructure
Tool projection revision
Cosmos client/configuration
WebApplicationDefinition de la aplicación operacional
```

y conectar allí:

```text
CosmosKpiDeliveryReader
→ AdaKpiCollector
→ attach_ada_kpi_collector
→ create_web_application
```

No inventar una app alternativa ni hardcodear Tool/Cosmos dentro de Generic Application.
