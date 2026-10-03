# Alarm Engine — Projection and Publication

Estado: **CURRENT implementation retained; direct Runtime→Delivery target SUPERSEDED; Runtime→Modeler→Delivery target DESIGN FROZEN / implementation PLANNED**.

## 1. Current implementation

CURRENT / VERIFIED:

```text
Runtime
    ├── CURRENT v1
    └── FACTS v2
            ↓
Delivery input receiver
```

Runtime outputs:

```text
runtime/output/current/latest.json
runtime/output/facts/facts-<hash>.json
runtime/output/state/facts-export-cursor.json
```

Delivery input maintains:

```text
delivery/input/current/latest.json
delivery/input/facts/facts-<hash>.json
delivery/input/state/facts-consumption-cursor.json
```

Esta implementación existe, está probada por los gates históricos registrados y no se elimina documentalmente hasta un cutover real.

## 2. Exact identity — FROZEN

```text
AlarmResolutionKey =
    (alarm_configuration_revision, confirmed_tool_catalog_revision)

Artifact pin =
    (source_key, result_id, manifest_sha256, resolution_key)

READY != EFFECTIVE
```

CURRENT, FACTS y cualquier configuración/estado downstream deben corresponder al artifact exacto EFFECTIVE.

## 3. CURRENT v1 — CURRENT implementation

`AlarmCurrentStatePublisher` publica la imagen operacional completa vigente de occurrences abiertas.

Incluye, entre otros:

```text
identity
occurrence_id
episode_id
started_at
evaluation
priority.disposition
priority.blockers
technical_hold
management_cycle/effect
deactivation_effect
pending_deactivation_request
assignments
pending_assignments
```

Es reemplazable y no conserva por sí sola toda la secuencia histórica.

## 4. FACTS v2 — CURRENT implementation

`AlarmCommittedFactsExporter` publica commits durables como batches inmutables y encadenados.

Cada batch conserva:

```text
batch_id
artifact_ref
journal_position
commit
commit_record_hash
previous_batch
records
sha256
```

Los records pueden incluir occurrence/episode, Journey, evidence, management/deactivation, assignment e input receipts.

FACTS v2 permite comprobar continuidad de la cadena exportada, pero no constituye firma criptográfica ni reemplaza políticas de retención externas.

## 5. Current Delivery input receiver — CURRENT, target role SUPERSEDED

El receptor actual:

```text
lee EFFECTIVE proyectado
valida exact materialization
stages CURRENT
recibe FACTS en orden
mantiene cursor propio
```

Como implementación existente permanece CURRENT.

Como frontera target:

```text
Runtime -> Delivery
```

queda SUPERSEDED.

El nuevo target es:

```text
Runtime
    ↓
Modeler
    ↓
Delivery
```

## 6. Runtime → Modeler target semantics — DESIGN FROZEN

Debe garantizar:

```text
durability
ordering
no-drop para cambios necesarios para reconstrucción/modelado
consumer-owned checkpoint
bounded consumption
Runtime producer never waits for Modeler acknowledgement
```

El Modeler debe poder caer, quedar atrasado y recuperarse sin detener Runtime.

### Exact physical contract — OPEN

No está congelado todavía si el Modeler:

```text
A) consume CURRENT v1 + FACTS v2 mediante un reader coordinado
```

o si Runtime publica adicionalmente:

```text
B) un model-input document/batch coherente por ciclo
```

No implementar esta elección por inferencia.

Cualquier solución debe evitar mezclar un FACTS antiguo con un CURRENT posterior sin una identidad temporal/artifact coherente.

## 7. Modeler → Delivery target semantics — DESIGN FROZEN

Modeler produce estado lógico final por destination/projection.

El handoff es:

```text
durable
latest-wins por destination
independent destination checkpoints
```

Ejemplo conceptual:

```text
projection A modeled revision = 45
projection A delivered revision = 40

Delivery publica 45 directamente.
No debe reproducir 41,42,43,44 sólo para alcanzar el estado vigente.
```

Un destino lento o fallido no debe bloquear otros destinos.

### Exact physical contract — OPEN

Queda por definir el documento exacto del modeled head y su cursor/checkpoint.

## 8. Modeler facts vs current head

Si se requiere auditoría de movimientos/rotaciones, esa historia puede persistirse separadamente.

No obligar a Delivery/Web a reproducir todos los estados intermedios.

Principio:

```text
historical modeled facts != mandatory visible frames
```

## 9. Web target

Web consume proyecciones ya modeladas.

Web no debe:

```text
recalcular carousel
recalcular queue-in-queue
mantener timers operacionales
reconstruir slots
leer WAL
```

Web sí decide la representación tecnológica/visual final:

```text
Dash components
CSS/theme
pixel geometry
rendering
```

## 10. Qualification / unverified

CURRENT historical gates de Runtime/Delivery continúan siendo evidencia del pipeline actual.

UNVERIFIED para el nuevo target:

```text
Modeler process
Runtime→Modeler physical handoff
Modeler durable state/recovery
Modeler→Delivery heads
Docker de cuatro jobs
multi-host storage/transport
production Azure behavior
```
