# Alarm Engine — Open Items

Estado: **CURRENT — Modeler boundary decided; materialization contract split is NEXT**.

## CLOSED / CURRENT decisions

```text
shared Tool contracts package
shared Alarm contracts package
Engine publication schemas ownership
Command Center semantic ownership before publication
READY != EFFECTIVE
exact artifact pin
Runtime CURRENT v1
Runtime FACTS v2
current direct Delivery receiver
target Runtime -> Modeler -> Delivery direction
Runtime/Modeler/Delivery same exact artifact
CAROUSEL six-position topology
QUEUE_IN_QUEUE Mine/Plant topology
backpressure ownership
Modeler recovery principle
no legacy adapters
no Engine dependency on Command Center Web
```

## NEXT único

```text
ADA-ALARM-ENGINE-MATERIALIZATION-CONTRACT-SPLIT
```

Required output:

```text
current DeliveryAlarmConfiguration field inventory
field-by-field ownership classification
RuntimeConfiguration final boundary
ModelerConfiguration exact contract
DeliveryConfiguration exact contract
codec/materialization impact
qualification plan
```

No implementar todavía scheduling operacional del Modeler.

## OPEN — ModelerConfiguration exact schema

Debe resolverse:

```text
cómo se identifica CAROUSEL vs QUEUE_IN_QUEUE
qué representa exactamente GENERIC / DISTRIBUTED
qué "variante" adicional existe y dónde vive
qué fields de Tool configuration se publican como primitives
qué enrichment metadata necesita realmente Modeler
```

No asumir valores que no estén en contrato/implementación o decisión explícita.

## OPEN — Runtime → Modeler physical handoff

Semántica congelada:

```text
durable
ordered
no-drop
consumer checkpoint
bounded memory
```

Elección física aún OPEN:

```text
coordinated CURRENT+FACTS reader
vs
explicit coherent model-input batch/document
```

Debe garantizar coherencia temporal/artifact y recovery.

## OPEN — ModelerState durable minimum

Definir exactamente qué persiste:

```text
input checkpoint
candidate state
scheduler cursors
visible/hidden membership
timer anchors
rotation counters only if contractually needed
current modeled heads
liveness/disconnection metadata
```

Evitar persistir datos derivables innecesariamente.

## OPEN — QUEUE_IN_QUEUE fairness

Topología congelada:

```text
MINE: 4 components / 3 visible
PLANT: 5 components / 3 visible
```

Pendiente:

```text
fairness dentro del mismo component
vs
fairness entre components
tratamiento de components vacíos/nuevos
selección después de desaparición abrupta
```

## OPEN — rotation window

El usuario indicó rango candidato:

```text
90..120 seconds
```

No hay valor final.

Debe ser configurable y validado; no hardcodear antes de decisión.

## OPEN — configuration adoption with live Modeler state

Cuando artifact/config cambia:

```text
A state/backlog -> B
```

definir:

```text
qué se preserva
qué se reconcilia
qué se reinicia
qué invalida state
cómo se evita mezclar artifacts
```

## OPEN — DeliveryConfiguration exact transport contract

Responsabilidad congelada:

```text
transport/publication only
```

Fields físicos exactos aún deben inventariarse contra provisioning/connections actuales.

## OPEN — residual `domain/alarms`

Dirección conceptual decidida:

```text
next_routing_tool_kind -> Command Center validation
ALARM_CONFIGURATION_SOURCE_KEY -> shared/primitive identity
```

Falta confirmar imports restantes antes de remover/rehome físicamente el package.

## OPEN — physical extraction / namespaces

Target scope:

```text
scopes/ada-alarm-engine
```

Aún no están congelados todos los package names / namespaces finales ni el orden físico exacto de movimiento.

No introducir aliases legacy.

## BLOCKED / SEPARATE

Full Command Center Web qualifier por:

```text
ada.web.tools.*
vs
ada.contracts.tools.*
```

No resolver dentro del Modeler/Alarm Engine contract split.

## PLANNED after contract split

```text
Modeler persistence/state
CAROUSEL implementation
QUEUE_IN_QUEUE implementation
Modeler -> Delivery heads
Delivery transport cutover
physical scope extraction
distribution/Docker qualification
```

Un frente por incremento.
