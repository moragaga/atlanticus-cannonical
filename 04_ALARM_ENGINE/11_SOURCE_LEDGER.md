# Alarm Engine — Source Ledger

Estado: **CURRENT**

## Implementation authority

```text
moragaga/atlanticus
branch: main
commit: 38379979fad90e2c514a2d56f3aa3889ceb71856
date: 2026-10-03T17:58:26Z
```

## Canonical source before replacement

```text
moragaga/atlanticus-cannonical
branch: main
commit: 8efd59431754059c548ed1e5d1263533b81012cd
date: 2026-10-03T14:15:02Z
```

## Relevant implementation surfaces verified

```text
scopes/ada-command-center/backend/processes/alarms-modeler/
scopes/ada-command-center/backend/processes/alarms-delivery/
scopes/ada-command-center/backend/processes/alarms-runtime/
scopes/ada-command-center/backend/processes/alarms-materialization/
scopes/ada-command-center/backend/alarms/materialization/
scopes/ada-command-center/backend/alarms/persistence/
scopes/ada-contracts/alarms/
```

Key files inspected in `main`:

```text
processes/alarms-modeler/.../job.py
processes/alarms-modeler/.../receiver.py
processes/alarms-modeler/.../projection.py
processes/alarms-delivery/.../job.py
processes/alarms-delivery/.../receiver.py
processes/alarms-delivery/.../parallel.py
processes/alarms-delivery/.../connections.py
processes/alarms-delivery/.../bootstrap.py
backend/pyproject.toml
```

## User-reported reproducible evidence

```text
Materialization ready.json + runtime.json + delivery.json + manifest.json
Runtime effective-head.json
Runtime CURRENT with ACTIVE/PREDOMINANT alarm
Runtime FACTS files
Modeler index + per-Tool latest.json
Delivery publish metrics
Cosmos direct read-back
```

## Historical decisions

`moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e` puede usarse como rationale histórico. No sustituye canonical ni current main.

## Evidence classification

Implementation source: **VERIFIED**.

Local E2E logs/read-back: **VERIFIED local**, no CI/Azure.

Advanced scheduler behavior: **UNVERIFIED / PLANNED**.
