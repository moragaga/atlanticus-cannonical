# Artifact and Distribution Boundary

Estado: **CURRENT CONTRACT / ADA DISTRIBUTED RUNTIME VERIFIED**

## Boundary

Web Distribution and Process Distribution are separate.

## ADA delivery strategy

```text
internal wheels
+
hash-pinned external runtime requirements
+
external Linux image build
```

Current evidence:

```text
internal wheels      73
ada-generic          0.2.26
project-tooling      0.1.1
Docker image build   PASS
runtime              PASS
```

## Independent consumer evidence

The generated ADA starter was instantiated in an independent consumer repository.

From that repository:

```text
docker build                  COMPLETED
compose up infra              COMPLETED
compose prepare web           COMPLETED
compose up web                COMPLETED
web container                 healthy
/health/live                  HTTP 200
/health/ready                 HTTP 200
```

Therefore the former:

```text
ADA-DISTRIBUTED-LINUX-RUNTIME-SMOKE
```

is CLOSED / VERIFIED.

## Cosmos Data Explorer

Local distributed Compose exposes built-in Cosmos Data Explorer on port 1234 and it returned HTTP 200.

## Qualification semantics

```text
PRECHECK_PASS
    preflight only

runtime VERIFIED
    requires actual image/runtime evidence
```

This distinction remains frozen even though ADA has now crossed the runtime gate.

## Host sync exception

Host `project.py sync` is not currently portable to macOS CPython 3.14.2 under binary-only resolution due `rcssmin==1.2.2`.

Classify:

```text
host sync macOS    BLOCKED
Docker/Linux       VERIFIED
```

Do not weaken binary-only reproducibility as an unreviewed workaround.

## Next distribution action

No immediate distribution redesign.

After the Tool-scoped/User cutover in product source:

```text
regenerate starter
build distribution
rerun isolated consumer smoke
```
