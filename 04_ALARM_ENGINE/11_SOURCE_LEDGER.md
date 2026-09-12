# Alarm Engine — Source Ledger

Estado: **AUDIT LEDGER**

Commit de decisiones auditado:
`ae28a733f9a973026182068af630a82ce39416bd`

Tree completo:
`9ea2223aa28502ae14929daece99ba75318aacc2`

## alarm_decisions

Preservados y clasificados:

- `Atlanticus_R3.6M_006B1_Canonical_Alarm_Definition_DESIGN_FROZEN.docx` — FROZEN.
- `Atlanticus_R3.6M_006B1_Canonical_Alarm_Definition_DRAFT_3.docx` — HISTORICAL DRAFT.
- `Atlanticus_R3.6M_006B2_Alarm_Projection_Boundary_DECISION_RECORDED.docx` — RECORDED.
- `Atlanticus_R3.6M_006B2_Alarm_Projection_and_Publication_Boundary_DECISION_RECORDED_INCREMENT_1.docx` — RECORDED.
- `Atlanticus_R3.6M_006B2_Alarm_Projection_and_Publication_Boundary_DECISION_RECORDED_INCREMENT_2.docx` — RECORDED.
- `R3.6M-006B.1-alarm-definition-contract-inventory-DESIGN-FROZEN.md` — FROZEN / preferred machine-readable authority.
- `R3.6M-006B.1-alarm-definition-contract-inventory-DRAFT.docx` — HISTORICAL.
- `R3.6M-006B.1-alarm-definition-contract-inventory-DRAFT_2.docx` — DUPLICATE of prior SHA.
- `R3.6M-006B.1-alarm-definition-contract-inventory-DRAFT.md` — HISTORICAL.
- `R3.6M-006B.1-alarm-definition-contract-inventory-DRAFT_2.md` — DUPLICATE of prior SHA.
- `R3.6M-006B.1-alarm-definition-contract-inventory-DRAFT_3.md` — HISTORICAL predecessor to frozen.
- `R3.6M-006B.2-alarm-projection-boundary-DECISION-RECORDED.md` — CURRENT semantic base.
- `R3.6M-006B.2-alarm-projection-and-publication-boundary-DECISION-RECORDED-INCREMENT-1.md` — CURRENT semantic extension.
- `R3.6M-006B.2-alarm-projection-and-publication-boundary-DECISION-RECORDED-INCREMENT-2.md` — CURRENT semantic extension.

## alarm_test — Contracts/checkpoints

Preservados explícitamente:

### E-008
- `Atlanticus_R35_E008_Source_Unavailable_CACHE_FALLBACK_Contract_v1.0.0.docx`

### E-009
- `Atlanticus_R35_E009_Invalid_Source_Candidate_Contract_v1.0.0.docx`

### E-010
- Aborted Run Corrective Checkpoint
- Closure Checkpoint
- Cumulative Harness Corrective Checkpoint
- Cumulative Harness Gate Checkpoint
- Lease Lost After WAL Before Cache Contract
- el archivo `(1)` del Lease Lost Contract es DUPLICATE byte-a-byte
- SMB Preflight Checkpoint

### E-011
- Adjudicator Corrective Checkpoint
- Cache Promotion Failure Contract
- Closure Checkpoint
- Harness Gate Checkpoint
- Harness Gate Corrective Checkpoint
- Harness Ready Checkpoint

### E-012
- Closure Checkpoint
- Drain Cancellation Product Finding
- Drain Under Workload Contract
- Harness Ready Checkpoint
- Product Fix Ready Checkpoint

### F-001
- Closure Checkpoint
- Harness Corrective Checkpoint
- Harness Gate Checkpoint
- Harness Ready Checkpoint
- Soak 500 Local 30m Contract

### F-002
- Closure Checkpoint
- Harness Ready Checkpoint
- Soak 1000 Local 30m Contract

### F-007
- Controlled Physical Dataset Bank Contract
- Dataset Capture Manifest Contract
- Docker Constrained Alarm Saturation Contract
- Docker Stress Harness Ready Checkpoint
- Harness Corrective Checkpoint
- Phase A Harness Corrective Checkpoint
- Physical Capacity Search Contract
- Physical Volume v2 Final Closed Checkpoint
- Real Volume v2 Contract
- JSON templates: dataset bank, dataset manifest, representativeness, synthetic conformance

### F-010
- Final Docker Qualification Closed Checkpoint
- Final Docker Qualification Contract
- Performance Campaign Final Closure / R36 Entry Decision

### Historical next-step
- `Atlanticus_R36A001_Azure_Runtime_Deployment_Contract_v1.0.0.docx` — HISTORICAL/SUPERSEDED AS ACTIVE NEXT STEP.

## Performance Campaign Planner

El repositorio conserva una secuencia acumulativa extensa desde:
- A001-A005;
- B001-B004 y B007-B011;
- C001-C008 con múltiples reruns/correctives de C004;
- D001-D010, incluyendo interrupted runtime lease, logical time, clock/routing/test/adjudication fixes;
- E001-E012;
- F001, F002 y F007 hasta `v1.0.135_F007_CLOSED_F010_CONTRACT_PROPOSED`.

Los XLSX intermedios son **genealogía/evidencia de campaña**, no decisiones canónicas independientes.

Duplicados conocidos por SHA:
- `...C004_R3_PRODUCT_FIX_READY (1).xlsx` == versión sin `(1)`.
- tres copias `...D002_CLOSED_D003_DESIGN_READY` comparten SHA.
- otros duplicados deben seguir detectándose por SHA durante la segunda pasada.

## Implementación actual relacionada

- `scopes/ada-command-center/backend/alarms/core`
- `scopes/ada-command-center/backend/alarms/persistence`
- `scopes/ada-command-center/backend/processes/alarms-runtime`
- `scopes/ada/web/alarms`

Este ledger no sustituye los artefactos originales. Su función es evitar que una fuente desaparezca silenciosamente durante la compactación.
