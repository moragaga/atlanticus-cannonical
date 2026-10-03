# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global — FROZEN

```text
uv; no pip normal
contracts before consumers
backend before frontend
clean root cutover
no legacy adapters/shims/aliases
one focus per increment
Git read-only unless explicit authorization
```

## Alarm pipeline — CURRENT / CLOSED baseline

El pipeline implementado vigente es:

```text
Command Center publication
    ↓
Alarm Configuration projection
    ↓
Materialization READY
    ↓
Runtime EFFECTIVE
    ↓
Runtime CURRENT + FACTS
    ↓
Modeler current projection
    ↓
Delivery
    ↓
Cosmos alarm-live-projection
    ↓
Web consumer [NEXT]
```

El consumo directo Runtime → Delivery queda **SUPERSEDED / REMOVED como frontera CURRENT**.

## Exact artifact invariant — FROZEN

```text
READY != EFFECTIVE
exact artifact = source_key + result_id + manifest_sha256 + resolution_key
Runtime, Modeler y Delivery usan el mismo exact artifact
no fallback to latest READY
```

## Materialization split — REFINED

Decisión previa:

```text
RuntimeConfiguration + ModelerConfiguration + DeliveryConfiguration
antes de implementar Modeler
```

Estado actual:

```text
RuntimeAlarmConfiguration + DeliveryAlarmConfiguration
```

El Modeler baseline consume ambos contratos existentes del mismo READY exacto.

La separación en `ModelerConfiguration` deja de ser prerequisito. Permanece OPEN sólo si aparece una responsabilidad/configuración independiente real al implementar scheduling avanzado.

## Runtime ownership — FROZEN

Runtime posee:

```text
evaluation
occurrence / episode
priority truth
management/deactivation operational effects
assignments/routing state
EFFECTIVE adoption
durable operational facts
```

Runtime no posee slots, carousel, QIQ ni transporte Cosmos.

## Modeler ownership — CURRENT baseline / advanced scheduling PLANNED

CURRENT:

```text
consume authoritative Runtime CURRENT
reopen exact materialized configuration
filter eligible PREDOMINANT active alarms
build per-Tool operator_pool
build first-six operator_view
persist current index + per-Tool snapshot
validate checksums/current exact pin
```

PLANNED:

```text
CAROUSEL
QUEUE_IN_QUEUE
rotation timers
fairness
durable scheduler checkpoint/state
staleness/disconnection policy
```

Modeler no recalcula prioridad.

## Runtime → Modeler handoff — REFINED

CURRENT baseline:

```text
Runtime CURRENT v1 + exact READY configurations
```

FACTS v2 no son requeridos por el Modeler baseline.

La semántica previa de handoff ordered/durable/no-drop permanece como target sólo para futuros cambios que realmente necesiten reproducir transiciones; no afirmar que está implementada hoy.

## Modeler → Delivery — CURRENT

Modeler publica un durable current head en filesystem:

```text
current/index.json
current/tools/<tool-hash>/latest.json
```

Delivery consume el head vigente y puede republicarlo idempotentemente.

No existe todavía checkpoint durable independiente por destination; esa optimización permanece OPEN si se requiere.

## Delivery — CURRENT / FROZEN

Delivery posee:

```text
Tool -> Cosmos connection resolution
bounded parallel publication
transport/upsert
publication metrics/errors
```

No posee modelado.

Contrato físico:

```text
container fijo = alarm-live-projection
partition key = /tool_key
```

`config/connections.json` se indexa por `tool_key` y sólo declara nombres de variables endpoint/database/credential.

Varios Tools pueden usar la misma conexión física.

## Live projection semantics — FROZEN baseline

`operator_pool` contiene alarms autoritativas elegibles para ese destino; no todas las evaluaciones ACTIVE.

`operator_view` es la selección actualmente visible del Modeler.

`ranking` no existe. `priority_order` es la prioridad ordinal vigente.

TRACE_ONLY no se publica como visible.

## Cause — OPEN

CURRENT snapshot conserva `cause_template` más evidence.

La materialización de una causa dinámica efectiva queda OPEN; Web no debe inventarla ni recombinar reglas de negocio por su cuenta.

## CAROUSEL / QIQ — DESIGN FROZEN parcial, NOT IMPLEMENTED

Se conservan las decisiones previas de 6 posiciones, reglas DISTRIBUTED y topología MINE/PLANT, pero siguen PLANNED hasta existir scheduler durable y qualification específica.

## NEXT único

```text
ADA-COMMAND-CENTER-ALARM-LIVE-WEB-CONSUMER
```
