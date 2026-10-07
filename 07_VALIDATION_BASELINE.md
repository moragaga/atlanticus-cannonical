# Atlanticus — Validation Baseline

Estado: **CURRENT — PROCESS DEPLOYMENT RESOURCE BOUNDARY QUALIFIED LOCALLY 2026-10-06**

## Autoridad

```text
Implementation  moragaga/atlanticus@5c40faed4df7f3d7b6db79251144a9ec09e09e91
Canonical base  moragaga/atlanticus-cannonical@15a51f70396726a2ad3b88d1afc66ce8cfff3300
```

## Process deployment gate

Ejecución reportada por el usuario:

```text
python3 tooling/gates/process-deployment/check.py
```

Resultado final:

```text
[1/7] Python runtime                         PASS
[2/7] deployment ownership/structure        PASS
[3/7] process container contracts           PASS
[4/7] Docker transport boundary             PASS
[5/7] Ruff check / format                    PASS
[6/7] deployment/tooling tests               PASS
[7/7] shell launchers                        PASS

Atlanticus process deployment flow validated
```

Suites:

```text
deployment/processes/tests                   31 passed
deployment/local/tests                       16 passed
tooling/tests/local/processes                 8 passed
tooling/tests/distribution/processes         56 passed
                                             ---------
total                                        111 passed
```

## Resource contract qualification

Automated coverage CURRENT demuestra:

```text
16 admitted Azure-style vCPU/RAM pairs
GiB -> Docker MiB conversion
invalid pair rejection
exact process/resource set validation
consumer resource override rendering
regeneration preserves consumer-owned sizing
pyproject resource declaration does not control distributed sizing
```

## Docker/local contract qualification

Automated coverage CURRENT demuestra:

```text
Dockerfile filtered process-root COPY contract
secrets.json admitted
config/connections.json admitted
.env excluded
config.json excluded
detail files excluded
local workspace preserves runtime inputs required by Dockerfile
```

## Extension integration evidence

General extension integration test CURRENT demuestra:

```text
new process is integrated
existing process configuration retained
runtime state retained
manifest/services updated
Compose structure regenerated
conflict rejected
publication failure rollback for existing tested point
```

Implementation also updates `deployment.resources.json` for new aliases.

Focused assertions specific to preserving custom existing resource values and rollback after resource-file publication are still missing.

## No acredita

```text
real Docker resource enforcement smoke after editing deployment.resources.json
Azure Container Apps deployment qualification
focused extension-resource atomicity qualification
legacy-distribution migration into the new resource contract
full artifact set qualification
.env.detail completeness
Python 3.14.7/Trixie migration
production Azure / Entra qualification
```

## Qualification scope

Esta qualification es local. No equivale a CI, Azure ni productivo.
