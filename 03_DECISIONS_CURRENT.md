# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global — FROZEN

```text
uv; no pip normal
contracts before consumers
backend before frontend
clean root cutover
no legacy adapters/shims/aliases
one focus per increment
Git read-only unless explicit authorization
```

Current distributed ADA remains Python 3.14.2 / slim-bookworm.
Python 3.14.7 / Trixie is separate `PLANNED`.

## Environment and persistence — FROZEN

```text
ATLANTICUS_ENVIRONMENT
ADA_PERSISTENCE_MODE
ADA_APPLICATION_NAMESPACE
ADA_TOOL_NAMESPACE
```

Emulator versus Azure is connection configuration, not architecture mode.

## Master Projection ownership — FROZEN

```text
atlanticus-web-master-projection
    reusable engine

ADA Generic
    ADA projection composition + provisioning

Command Center Generic
    Command Center projection composition + provisioning
```

Do not duplicate the reusable engine inside products.

## Command Center

This hito does not change Command Center contracts.

Its distributed runtime qualification and Alarm integration remain separate work.

## ADA physical durable contract — CURRENT

```text
ADA_STORAGE_CONTAINER_NAME
ADA_STORAGE_CONNECTION_STRING

or

ADA_STORAGE_ACCOUNT_URL
ADA_STORAGE_SAS_TOKEN

ADA_COSMOS_ENDPOINT
ADA_COSMOS_KEY
ADA_COSMOS_DATABASE_NAME
```

`dataproduct` remains the default Storage container.

## ADA namespace ownership — REFINED / DECIDED

`ADA_APPLICATION_NAMESPACE=conciencia_situacional` is the global application boundary.

`ADA_TOOL_NAMESPACE` identifies the current Tool scope.

DECIDED target:

```text
application-global
    Users identity registry

tool-scoped
    Tool Configuration
    Profiles
    Navigation
    ADA Access
    Operational
    Tool User Membership
    KPI Registry
    KPI Definitions
    Tool Users Recovery Snapshot
```

Implementation has not completed this cutover.

## Users model — REFINED / DECIDED

SUPERSEDED target:

```text
global UserRecord owns profile_key + enabled
```

DECIDED target:

```text
global user
    identity/global personal data only

tool membership
    user_id + profile_key + enabled

tool Cosmos users-runtime
    complete session snapshot
```

Operational shape in runtime snapshots is stable and nullable, not omitted.

## Users recovery — REFINED

Immediate recovery:

```text
Tool Users Recovery Snapshot
→ users-runtime
```

Granular reconstruction from independent durable contracts is `PLANNED / FUTURE`, not part of the next increment.

## Navigation authorization — REFINED / DECIDED

The current ambiguity:

```text
allowed_profiles=[]
→ public
```

cannot represent a privileged-only route.

Target behavior:

```text
PUBLIC
RESTRICTED + []
    root/local only
RESTRICTED + [profiles]
    selected profiles + root/local
```

Exact persisted schema may be finalized during contract implementation, but this behavior is frozen.

## KPI Registry / Delivery — REFINED / DECIDED

KPI Registry projection/materialization will carry the stable Tool identity:

```text
tool_key
```

derived from Tool Projection.

Do not make Tool display name an independently editable KPI authority.

## Distributed runtime — CLOSED

`ADA-DISTRIBUTED-LINUX-RUNTIME-SMOKE` is `CLOSED / VERIFIED`.

`PRECHECK_PASS != runtime verified` remains a general rule.

## Next order — FROZEN

```text
1. ADA Tool-scoped configuration + Users runtime cutover
2. regenerate/distribute consumer
3. resume real ADA configuration
4. validate recovery on the corrected ownership model
5. continue UI / Collector / operational runtime
6. integrate Alarm surfaces/backend in their dedicated focus
```

Do not pull Alarm, Collector or Python migration into step 1.
