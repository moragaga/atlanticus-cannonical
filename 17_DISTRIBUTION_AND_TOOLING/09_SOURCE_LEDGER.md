# Distribution and Tooling — Source Ledger

Estado: **AUDIT LEDGER — ADA DISTRIBUTED RUNTIME CLOSED 2026-10-02/03**

## Authorities

```text
Implementation
moragaga/atlanticus@38bcd8c5607d67f999e2bc4bf9dbf176c8340588

Decisions
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e

Canonical before replacement
moragaga/atlanticus-cannonical@0a2ff691d8bb9a4bfe743cadb1cacb9c83d865c7
```

Git remained read-only from the assistant side.

## Prior artifact baseline retained

```text
ada-generic-application==0.2.26
ada-project-tooling==0.1.1
internal wheels=73
delivery strategy=internal-wheels-external-image-build
```

## Cosmos Data Explorer increment

Verified:

```text
ADA distribution test suite      63 passed
starter infra/full               ENABLE_EXPLORER=true
local bind                       127.0.0.1:${ADA_COSMOS_EXPLORER_PORT:-1234}:1234
distribution build               73 wheels
precheck                         PRECHECK_PASS
Explorer runtime                 HTTP 200
```

No `.env.detail` application variable was added for the Explorer port.

## Isolated consumer runtime

User evidence from `mlp0002-code-aa-ada-webapp-3`:

```text
docker build --tag ada-generic:compose-local
    COMPLETED

compose up infra
    COMPLETED

compose ps infra
    Azurite 127.0.0.1:10000
    Cosmos 127.0.0.1:1234,8080,8081

compose prepare web
    Blob dataproduct CREATED
    Cosmos cosmosdb-ada CREATED
    six Cosmos containers CREATED

compose up web
    COMPLETED

compose ps web
    healthy

/health/live
    HTTP 200
    status alive
    version 0.2.26

/health/ready
    HTTP 200
    status ready
    checks {}
```

Classification:

```text
ADA distributed Linux runtime     VERIFIED / CLOSED
consumer repository independence  VERIFIED
readiness dependency checks       UNVERIFIED
```

## Host sync finding

`project.py sync` on macOS failed under binary-only resolution:

```text
rcssmin==1.2.2
no usable wheel for tested macOS CPython 3.14
```

Current code declares the dependency directly and sync uses binary-only external resolution.

Classification:

```text
VERIFIED / BLOCKED / non-blocking for Docker runtime
```

## Configuration findings after runtime closure

Current code and runtime evidence show:

```text
application_source
    Navigation
    Profiles
    ADA Access
    Operational

tool_source
    Tools
    KPI Registry
    KPI Definitions

Users Registry
    application-global
```

Real Tool Projection observed:

```text
tool_key      tool_operaciones_integradas_af1b7d9983bd
display_name  Operaciones Integradas
```

Real KPI Registry Projection observed exact dependency on Tool Projection.

These findings triggered the next product ownership cutover.

## Decisions accepted in this hito

`DECIDED / PLANNED`, not implemented:

```text
global Users identity drops profile_key/enabled
Tool User Membership owns profile_key/enabled
Navigation/Profiles/Access/Operational move to Tool Source root
users-runtime becomes complete Tool session snapshot
operational runtime shape remains present with nullable values
Tool Users Recovery Snapshot is immediate recovery path
granular rebuild joins are future only
Navigation gains explicit public/restricted semantics
KPI Registry projection/materialization exposes tool_key
```

## Next audit boundary

```text
ADA-TOOL-SCOPED-CONFIGURATION-AND-USER-RUNTIME
```

Do not mix UI/Alarm/Collector work into the root cutover.
