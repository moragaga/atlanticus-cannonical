# Distribution and Tooling — Canonical Index

Estado: **PLANNED / NEXT — FINAL ARTIFACT AND DISTRIBUTION QUALIFICATION**

Operational Data and KPI consumer-contract normalization are closed.

Distribution and tooling is now the next single focus.

## Order

```text
1. regenerate current-head artifacts
2. verify every expected artifact is generated
3. qualify artifact contents/installability
4. audit every .env.detail entry
5. classify required/optional/default/system-derived/system-assigned/secret fields
6. generate final distribution
7. run isolated consumer qualification
```

## Scope constraints

Do not mix:

```text
Alarm Runtime migration
Python migration
production Azure/Entra
unrelated Web/product work
```

Alarm Runtime is intentionally BLOCKED by the Operational Data cutover and will be migrated in a later dedicated increment.

## Existing documents

`01_BACKEND_GENERATION.md` remains the ownership direction.

`06_ENV_DETAIL.md` remains the audit contract and must be exercised in the next focus.
