# Scripts and Integrity Validation

Estado: **CURRENT — PROCESS DEPLOYMENT GATE GREEN**

## Process deployment gate

CURRENT gate:

```text
tooling/gates/process-deployment/check.py
```

Responsibilities:

```text
Python runtime validation
deployment/tooling structure
exportable process contracts
Docker runtime-input boundary
Ruff / format
focused deployment/tooling tests
launcher syntax
```

## Current qualification

Reported GREEN:

```text
deployment/processes/tests                   31 passed
deployment/local/tests                       16 passed
tooling/tests/local/processes                 8 passed
tooling/tests/distribution/processes         56 passed

total                                        111 passed
```

Ruff and format passed after gate formatting.

Shell launchers:

```text
tooling/local/processes/process.sh
tooling/distribution/processes/distribute.sh
tooling/distribution/processes/consumer/process.sh
tooling/gates/process-deployment/check.sh
```

all passed `sh -n`.

## Resource tests CURRENT

Protect:

```text
all admitted resource pairs
invalid-pair rejection
Docker MiB translation
exact alias/resource set
resource override rendering
regeneration preservation
pyproject resources ignored
```

## Docker transport test refinement

SUPERSEDED rule:

```text
secrets.json must never reach process image
```

CURRENT:

```text
allowed
    secrets.json
    config/connections.json

excluded
    .env
    config.json
    *.detail
```

The local workspace and Docker allowlist must stay aligned.

## Next validation gap

Add focused extension/resource tests for:

```text
custom existing sizing preservation
new alias default
malformed resource contract no-mutation
rollback after resource-file publication
```

Do not add implementation-structure tests as substitutes for these behavior contracts.

## Master Integrity Gate

Repository-wide master gate remains a separate planned capability.

The process-deployment gate is not the repository master gate.
