# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Checkpoint CURRENT verificado para este cierre:
  `d484569cbe0290f38f239481cde81b13a23deecf`
- Parent inmediato verificado:
  `dde1e3a114a04b22cc2118c347a7ed907852c06b`
- Tree verificado:
  `4c7c8209f2d0c670d3c6e8b5185b5af12172e591`
- Fecha del commit:
  `2026-09-21T01:04:47Z`

Estado acumulado relevante:

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
```

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Checkpoint inspeccionado antes de este reemplazo:
  `a8c8c80ed3392cb189923d00bd5037e5965e2da5`

`atlanticus-cannonical:main` es autoridad documental vigente, subordinada a
`atlanticus:main` cuando la implementación publicada demuestra un estado posterior.

## Referencias históricas

`moragaga/atlanticus-decisions` permanece **HISTORICAL**.

Checkpoint observado:

```text
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

Durante este cierre sólo se inspeccionó su árbol para localizar referencias históricas. No se
auditaron los contenidos binarios de decisiones antiguas. Por tanto, cualquier conflicto
específico nuevo con esas decisiones permanece **UNVERIFIED**.

## Jerarquía

1. `atlanticus:main`: realidad implementada.
2. `atlanticus-cannonical:main`: contracts, fronteras, roadmap y estado vigente.
3. Qualification y tests vigentes: evidencia de propiedades demostradas.
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

## Collector KPI CURRENT

Capability:

```text
scopes/ada/web/kpis/collector
ada-web-kpi-collector==0.1.0
```

Contrato CURRENT:

```text
Latest polling      = 10 s default, configurable
Timeseries polling  = 120 s default, configurable
Browser cache read  = 10 s default, configurable
Latest priority     = before Timeseries when both are due
```

El collector:

- consume Latest y Timeseries mediante `KpiDeliveryReader`;
- mantiene cache in-process por worker;
- usa snapshots inmutables por Component;
- conserva Latest y Timeseries como superficies independientes;
- no exige atomicidad cross-document;
- impide regresión de `watermark_utc` y `end_utc`;
- valida compatibilidad por `configuration_revision` + `tool_projection_revision`;
- mantiene un store lógico por `ToolComponent`;
- no crea stores por Subcomponent;
- el browser lee cache, nunca Cosmos;
- `/health/`, `/assets/` y `/.auth/` no arrancan el poller;
- el primer request real inicia el poller del worker;
- source failure no rompe el request ni reemplaza estado bueno por estado inválido.

El contrato Web permite adjuntar la capability a una definición ya resuelta mediante:

```text
attach_ada_kpi_collector(WebApplicationDefinition, collector)
```

La Generic Application no depende obligatoriamente del collector.

## Web Observability CURRENT

Atlanticus Web registra la misma instancia `WebObservability` del runtime como servicio:

```text
WEB_OBSERVABILITY_SERVICE_KEY = atlanticus.web.observability
```

El collector la consume como dependencia declarada.

Política CURRENT:

```text
KpiDeliveryReadError       -> WARNING deduplicado por source+signature
KpiCollectorContractError -> ERROR deduplicado por source+signature
otro refresh failure      -> ERROR deduplicado por source+signature
runtime thread failure    -> CRITICAL
refresh válido            -> limpia incidente de esa source
```

No emitir telemetría por polling exitoso normal.

## Siguiente foco único

```text
ADA-GENERIC-COLLECTOR-OPERATIONAL-INTEGRATION
PLANNED / NEXT
```

El siguiente incremento debe **integrar el collector ya cerrado** en la composición operacional
real que conoce la Tool y sus conexiones. No rediseñar el collector ni crear una aplicación
paralela para evitar buscar la composición existente.

Debe partir de las fuentes autoritativas y resolver, con el gap mínimo:

```text
ToolConfiguration / ToolStructure CURRENT
Tool projection revision CURRENT
Cosmos client/configuration CURRENT
AdaKpiCollector
attach_ada_kpi_collector
existing ADA application composition root
```

No abrir en paralelo Python metadata, KPI Inspection, CSS, Identity, Manager ni nuevos contratos.
