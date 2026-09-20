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

## Execution refinement — 2026-09-20 — Users Administration V1

Checkpoint:

```text
moragaga/atlanticus@ce07ada07e3f4f100b97ad2ac5e7285b54419c20
```

Users Administration closes its current V1 blocking flow without converting Users into a generic
Manager Source/Projection module.

CURRENT:

```text
Users
→ ManagerEntry

Users synthetic Source/Projection
FORBIDDEN
```

The UI is split into two internal views:

```text
Usuarios
Por promover
```

`Usuarios` is the default view.

Promotion remains intentionally singular:

```text
candidate
→ choose assignable profile
→ Promover
→ immediate administrative commit
```

Existing managed users use:

```text
Editar
→ profile / enabled
→ Guardar
→ immediate administrative commit
```

No global Users draft/publication workflow is introduced.

Profile boundary refined from implementation findings:

```text
guest
valid transient/pending UserRecord profile
not administratively assignable

local
runtime-local only
not administratively assignable

basic / root / configured profiles
administratively assignable
```

An intermediate attempt to reject `guest` in `normalize_managed_profile_key()` was superseded because
it invalidated legitimate pending `UserRecord` state.

CURRENT enforcement is at the assignment boundary:

```text
require_managed_profile('guest')
→ reject

available_managed_profiles()
→ exclude guest + local
```

The UI review is accepted for the current V1 with minor non-blocking polish deferred.

The final automated requalification after the Guest-boundary corrective remains UNVERIFIED and must
not be recorded as GREEN without a later test result.

The production Blob/Cosmos wiring for Users Administration also remains a separate qualification.
