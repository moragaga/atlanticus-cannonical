# ADA Web — Infrastructure Startup

Estado: **CURRENT — DISTRIBUTED LOCAL RUNTIME VERIFIED / READINESS DEPENDENCY CHECKS OPEN**

## Verified distributed runtime

The generated ADA distribution was instantiated in an independent consumer repository and executed through Docker.

Evidence:

```text
image build                          COMPLETED
python image                         python:3.14.2-slim-bookworm
Azurite                              RUNNING
Cosmos Emulator                      RUNNING
Cosmos Data Explorer                 HTTP 200 on 127.0.0.1:1234
```

Resource preparation created:

```text
Blob container       dataproduct
Cosmos database      cosmosdb-ada
Cosmos containers    6
```

Web:

```text
container            healthy
/health/live         HTTP 200
/health/ready        HTTP 200
version              0.2.26
```

## Data Explorer

Local Compose CURRENT exposes Cosmos built-in Data Explorer:

```text
ENABLE_EXPLORER=true
127.0.0.1:${ADA_COSMOS_EXPLORER_PORT:-1234}:1234
```

This is local operational tooling, not an application `.env.detail` variable.

## Readiness limitation

Current body observed:

```json
{"checks": {}, "status": "ready"}
```

Therefore:

```text
endpoint availability     VERIFIED
dependency readiness      NOT PROVEN BY checks
```

Keep this OPEN but non-blocking for the current Tool-scope cutover.

## Host sync macOS limitation

`tooling/project.py sync` is BLOCKED on macOS CPython 3.14.2 because binary-only resolution requires `rcssmin==1.2.2`, which has no usable macOS CPython 3.14 wheel in the tested contract.

Docker/Linux build remains verified and is the current supported path for this gate.

## Current startup rule

For local distributed validation:

```text
docker build
compose up infra
compose prepare web
compose up web
```

Keep infra/prepare/web separable because recovery tests intentionally destroy only projection infrastructure.

## Recovery gate

Do not execute the final destructive Cosmos recovery proof until Tool-scoped configuration/User ownership is corrected.
