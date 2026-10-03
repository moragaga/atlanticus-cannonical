# Web Platform — Open Items

Estado: **CURRENT — ADA TOOL-SCOPED CONFIGURATION/USERS CUTOVER NEXT**

## CLOSED / VERIFIED

```text
CURRENT-HEAD-DISTRIBUTION-REGENERATION
ADA-LOCAL-COSMOS-DATA-EXPLORER
ADA-DISTRIBUTED-LINUX-RUNTIME-SMOKE
ADA-CONSUMER-REPOSITORY-RUNTIME
```

## OPEN / NEXT

```text
ADA-TOOL-SCOPED-CONFIGURATION-AND-USER-RUNTIME
```

Acceptance boundary:

```text
Navigation/Profiles/Access/Operational use Tool Source root
global Users identity no longer owns profile_key or enabled
Tool User Membership owns profile_key + enabled
Tool users-runtime is complete login/session snapshot
operational runtime shape is stable with nullable values
Tool Users Recovery Snapshot can restore users-runtime
Navigation can represent privileged-only and profile-restricted routes
KPI Registry projection/materialization exposes tool_key
no legacy storage path/model adapters
```

## OPEN / AFTER

```text
regenerate distribution
rerun consumer runtime
resume Operaciones Integradas configuration
Master Projection recovery gate
Tool hot refresh
Time Status runtime
KPI Delivery
UI
Alarm integration
```

## OPEN / SEPARATE

```text
macOS host sync rcssmin wheel
/health/ready functional checks
production Entra/Azure
Command Center distributed runtime
Python 3.14.7/Trixie
```

Do not mix AFTER/SEPARATE items into the next root cutover.
