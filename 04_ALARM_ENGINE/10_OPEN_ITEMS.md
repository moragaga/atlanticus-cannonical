# Alarm Engine — Open Items

Estado: **CURRENT — physical extraction design is NEXT**.

## CLOSED / CURRENT

```text
shared Tool contracts package
shared Alarm contracts package
Engine publication schemas ownership
Command Center Alarm/Tool consumer cutover
Command Center Users/Profiles/Navigation/Manager parity
Command Center Web lock normalization
```

## NEXT único

```text
ADA-ALARM-ENGINE-EXTRACTION-DESIGN
```

Required outputs before implementation:

```text
complete backend package inventory
dependency graph
KEEP / MOVE / REMOVE / INVERT / REHOME classification
Command Center publication -> Engine input contract
destination of ada-command-center/domain/alarms
target scopes/ada-alarm-engine layout
package/module rename strategy
incremental qualification order
```

## Working hypothesis

```text
all current ada-command-center/backend Alarm packages are Engine;
Web-facing acquisition/resolution layers are the part to remove/invert.
```

This remains PROPOSED until verified package by package.

## Known debt to remove

At least `alarms-materialization-process` still depends on Web packages for configuration/projection acquisition.

Target Engine must not depend on Command Center Web.

## BLOCKED / SEPARATE

Full Command Center Web qualifier:

```text
catalog            FAIL
discovery-cosmos   FAIL
```

Cause observed: duplicated Tool contract types between ADA Web and `ada-contracts-tools`.

Do not patch Alarm contracts for this.

## PLANNED / AFTER EXTRACTION DESIGN

- physical move/rename to `scopes/ada-alarm-engine`;
- deterministic Materialization cleanup;
- distribution/build qualification;
- independent Docker jobs;
- Live Projection;
- History/Analytics;
- production Azure/Entra validation.

## Frozen invariants

```text
contracts before consumers
READY != EFFECTIVE
exact artifact pin
Runtime and Delivery same exact artifact
no fallback to latest READY
no legacy adapters by inference
no Tool semantic rediscovery in target Materialization
no Engine dependency on Command Center Web
```
