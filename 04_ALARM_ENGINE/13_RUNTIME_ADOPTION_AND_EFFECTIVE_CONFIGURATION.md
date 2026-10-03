# Alarm Engine — Runtime Adoption and Effective Configuration

Estado: **CURRENT — RUNTIME/EFFECTIVE + MODELER/DELIVERY EXACT-PIN CONTINUITY IMPLEMENTED**

## Exact adoption invariants — FROZEN

```text
READY != EFFECTIVE
AlarmResolutionKey = (alarm_configuration_revision, confirmed_tool_catalog_revision)
Exact artifact ref = (source_key, result_id, manifest_sha256, resolution_key)
```

Materialization crea READY; Runtime adoption otorga autoridad operacional al exact artifact.

No fallback to latest READY.

## Runtime CURRENT

Runtime reabre exact materialization, adopta EFFECTIVE y publica CURRENT/FACTS.

`runtime/state/effective-head.json` es la projection recuperable de esa autoridad.

## Modeler continuity CURRENT

Antes de modelar:

```text
read EFFECTIVE
read Runtime CURRENT
require CURRENT.artifact_ref == EFFECTIVE.target_artifact_ref
read exact READY by result_id + manifest_sha256
require runtime/delivery resolution_key consistency
```

Si no coincide, Modeler espera o aborta; no mezcla artifacts.

## Delivery continuity CURRENT

Delivery:

```text
reads EFFECTIVE
reads Modeler index/snapshots
requires Modeler artifact_ref == EFFECTIVE exact pin
reopens exact READY
validates resolution identity
```

Luego publica el snapshot ya modelado.

## Direct Runtime → Delivery

La frontera directa anterior queda **SUPERSEDED**. Delivery CURRENT consume Modeler, no Runtime CURRENT/FACTS directamente.

## Runtime → Modeler semantics CURRENT

Baseline físico:

```text
CURRENT v1 current-head
+
exact READY configuration
```

La semántica ordered/no-drop con checkpoint durable no está implementada en este baseline y sólo debe introducirse si futuros requisitos necesitan transiciones completas.

## Artifact A → B with scheduler state — OPEN

Runtime adoption exacta ya existe.

Cuando Modeler tenga timers/colas durables debe definirse explícitamente qué estado se preserva/reconcilia/reinicia al cambiar artifact.
