# Alarm Engine — Decision Index

Estado: **CANDIDATE**

| ID | Tema | Estado | Fuente principal |
|---|---|---|---|
| ALARM-DEF-B1 | Canonical Alarm Definition | DESIGN FROZEN | `R3.6M-006B.1-alarm-definition-contract-inventory-DESIGN-FROZEN.md` |
| ALARM-PROJ-B2-BASE | Live vs Management Projection | DECISION RECORDED | `R3.6M-006B.2-alarm-projection-boundary-DECISION-RECORDED.md` |
| ALARM-PROJ-B2-I1 | Publication/runtime/delivery boundary | DECISION RECORDED | `...INCREMENT-1.md` |
| ALARM-PROJ-B2-I2 | Latest Saved = Latest Valid | DECISION RECORDED | `...INCREMENT-2.md` |
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

El frozen es autoridad del contrato, no los drafts.

## Genealogía B.2

- Base DECISION RECORDED.
- Increment 1 amplía publicación/materialización/adopción.
- Increment 2 endurece persistencia con `LATEST SAVED = LATEST VALID`.

No tratar los tres como alternativas mutuamente excluyentes; el estado actual es acumulativo salvo binding de storage supersedido.
