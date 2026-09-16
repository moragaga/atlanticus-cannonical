# Baseline Evolution

## Baseline 1.0 — 2026-09-12

Promotes Candidate V5 to the first canonical execution baseline.

Key final adjustments:

- User Activity consolidates by user/session/page.
- session survives full reload.
- five-minute watcher/visibility semantics preserved.
- partition keys deferred until workload/resource trace.
- KPI `REPROCESS_CURRENT` uses same per-job variable.
- resources/containers deferred until complete Tool trace.
- pre-Manager becomes authenticated Login/Bootstrap Console.
- base projections independent; derived resolutions carry actual dependencies.
- Command Center analytics deferred until data sufficiency qualification.
- first Tool = Operaciones Integradas.
- second Tool = Mina.
- backend/frontend generator outputs must be distribution-ready.
- Atlanticus explicitly does not own corporate DevOps pipeline.
- scripts by component + master integrity gate.
- Cosmos/Storage support-service declaration.
- env.detail enriched with meaning/rationale.
- real READMEs after stabilization.
- loaders required for product close.
- Manager ADA Component External Links + JS popover + warmup.
- Alarm Management frontend follows Backend authority.
- Atlanticus University added for isolated real teaching cases.

From this baseline forward, architecture changes require a concrete implementation finding or product need.

## Execution refinement — 2026-09-16

Concrete implementation of KPI Configuration established a real semantic projection dependency that refines one Baseline 1.0 statement.

Historical formulation:

```text
base projections independent; derived resolutions carry actual dependencies
```

Refined CURRENT rule:

```text
no artificial bootstrap dependencies
exact ProjectionTarget.dependencies when a projection has a real semantic dependency
derived resolutions remain for genuinely derived composition
```

Verified implemented case:

```text
Tool ProjectionTarget
    ↓
KPI Configuration ProjectionTarget.dependencies
```

This refinement does not introduce a global projection order or a new coordinator.

It also does not change ownership:

```text
Tools             → scopes/ada
KPI Configuration → scopes/ada
KPI Definition    → scopes/ada
```

These ADA-specific capabilities consume generic Atlanticus infrastructure without becoming generic core capabilities.

Checkpoint establishing the KPI Configuration cutover:

```text
moragaga/atlanticus@4c7f8aa8b541e8b8f8abc7b49fe22526a4952bfe
```
