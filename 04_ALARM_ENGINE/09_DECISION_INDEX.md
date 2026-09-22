# Alarm Engine — Decision Index

Estado: **CURRENT / HISTORICAL SOURCES + PROJECT REFINEMENTS + B.2 RESOLVER CURRENT**

| ID | Tema | Estado | Fuente principal |
|---|---|---|---|
| ALARM-DEF-B1 | Canonical Alarm Definition historical base | DESIGN FROZEN / HISTORICAL INPUT | `R3.6M-006B.1-alarm-definition-contract-inventory-DESIGN-FROZEN.md` |
| ALARM-PROJ-B2-BASE | Live vs Management Projection | DECISION RECORDED / HISTORICAL INPUT | `R3.6M-006B.2-alarm-projection-boundary-DECISION-RECORDED.md` |
| ALARM-PROJ-B2-I1 | Publication/runtime/delivery boundary | DECISION RECORDED / HISTORICAL INPUT | `...INCREMENT-1.md` |
| ALARM-PROJ-B2-I2 | Latest Saved = Latest Valid | DECISION RECORDED / HISTORICAL INPUT | `...INCREMENT-2.md` |
| ALARM-B2-PURE-RESOLVER | Pure deterministic configuration resolver | CURRENT / IMPLEMENTED | `9398786ae9af7c00de1bcca9d7a311fe9ef2155f` |
| ALARM-B2-C2-ROUTING | Relative waits -> cumulative absolute Runtime offsets | CURRENT / IMPLEMENTED / TESTED | resolver + `test_resolver.py` |
| ALARM-B2-VISUAL-TARGETS | Process/IO/Strategic visual qualification | CURRENT / IMPLEMENTED / TESTED | resolver + `test_resolver.py` |
| ALARM-RUNTIME-VISIBILITY | Runtime visibility removal | CURRENT / IMPLEMENTED | `PlannedAlarm.delivery_enabled` + `PriorityDisposition.SHADOW` removed |
| ALARM-RANK-SUPPRESSION | Uniform suppression by priority_order | CURRENT / IMPLEMENTED / TESTED | Alarm Core Management/Priority |
| ALARM-DEACTIVATION-CASCADE | Active deactivation sustains lower-rank cascade | CURRENT / IMPLEMENTED / TESTED | `d48abf17689e7dd8ef93827415b71b7b6be4385b` |
| ALARM-REAPPEARANCE-SECONDS | Runtime timer field | CURRENT / IMPLEMENTED / TESTED | `cd08bd8d2c25bd89eb39fa15cbda209c8e9be617` |
| ALARM-DURABILITY | WAL -> durable -> snapshots -> materialized | IMPLEMENTED + VALIDATED | persistence `store.py`, recovery/fencing tests |
| ALARM-FENCING | stale writer cannot complete after takeover | IMPLEMENTED + VALIDATED | `test_fencing.py`, E-010 |
| ALARM-RECOVERY | replay/discard/fail-closed by durable authority | IMPLEMENTED + VALIDATED | `test_recovery.py` |
| ALARM-E011 | empty reset snapshot adjudication | CLOSED HARNESS FINDING | E-011 corrective |
| ALARM-E012 | drain cancellation behavior | CLOSED PRODUCT FINDING/FIX | E-012 finding + fix + closure |
| ALARM-F010 | final constrained Docker qualification | CLOSED PASS/GREEN | F-010 closure |

## Genealogía B.1

- DRAFT `.docx`
- DRAFT_2 `.docx`: **DUPLICATE byte-a-byte** del DRAFT.
- DRAFT `.md`
- DRAFT_2 `.md`: **DUPLICATE byte-a-byte** del DRAFT markdown.
- DRAFT_3
- DESIGN_FROZEN

El frozen preserva la base histórica del contrato, pero no reemplaza refinamientos posteriores ya
implementados y canonizados.

## Refinamientos CURRENT respecto de B.1

### Special Cascade

B.1:

```text
managed predominant Special Condition
-> suppress all other active Rules in group
```

CURRENT:

```text
source con effect causal vigente
-> suppress sólo active targets con priority_order mayor
```

La regla de ranking aplica uniformemente; Special Condition no tiene una liga separada.

### Deactivation cascade

CURRENT agrega una precisión no expresada en B.1:

```text
active DeactivationEffect
-> mantiene lower-rank cascade suppression
-> aunque ManagementEffect haya terminado
```

### Runtime reappearance unit

B.1 authored:

```text
ReappearanceDefinition.after_minutes
```

CURRENT Runtime:

```text
PlannedAlarm.reappearance_after_seconds
```

CURRENT resolver materializa:

```text
None -> None
M -> M * 60
```

### Message activation

CURRENT:

```text
inactive Message
-> definición válida
-> no seleccionable para nuevas gestiones
```

Si una formulación histórica exige Message activo para validez, queda SUPERSEDED.

### Routing C2

El authoring conserva:

```text
wait_minutes_from_previous_step
```

CURRENT resolver congela:

```text
enabled waits relativos
-> acumulación por step_order
-> RoutingDestination.delay_seconds absoluto desde occurrence start
```

Steps disabled:
- no contribuyen al tiempo ejecutable;
- siguen sujetos a Tool reference qualification.

Esta precisión reemplaza cualquier lectura ambigua de waits independientes.

### Visual targets

Historical B.1 no cerraba comportamiento Alarm para Strategic.

CURRENT resolver congela:
- Process requiere `process_projection_mode`;
- Integrated Operations lo prohíbe;
- Strategic visual target queda BLOCKED mientras Alarm projection siga indefinida.

No inferir de esto una semántica futura Strategic; sólo es el comportamiento de qualification CURRENT.

## Genealogía B.2

- Base DECISION RECORDED.
- Increment 1 amplía publicación/materialización/adopción.
- Increment 2 endurece persistencia con `LATEST SAVED = LATEST VALID`.
- Project CURRENT implementa el pure resolver sobre los contratos materialization ya existentes.

No tratar las fuentes históricas como alternativas mutuamente excluyentes; el estado es acumulativo
salvo contrato explícitamente supersedido.

## Regla de autoridad

Ante diferencia:

```text
atlanticus:main CURRENT
> canonical CURRENT
> historical decisions
```

Exponer el conflicto; no retroceder implementación para hacerla coincidir con una decisión histórica.
