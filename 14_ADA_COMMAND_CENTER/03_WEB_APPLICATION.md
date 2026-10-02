# ADA Command Center — Web Application

Estado: **CURRENT — GENERIC PRODUCT ROOT IMPLEMENTED / DISTRIBUTED / LOCAL RUNTIME ONLY**

## Product roles

```text
ada-command-center-generic-application
→ real product composition root

ada-command-center-configuration-manager
→ separate development/testing/qualification application
```

## Local product host

CURRENT runtime:

```text
ManagerConfigurationReader
→ open_local_application
→ LocalIdentityProvider
→ local Manager
→ create_application
→ run_web_application
```

The host fails fast for production and for a non-local Manager provider.

## Storage

Tool Catalog uses configured Storage even with local Manager.

Current `.env.detail` includes:

```text
ADA_COMMAND_CENTER_STORAGE_CONNECTION_STRING
ADA_COMMAND_CENTER_STORAGE_CONTAINER_NAME
```

## Cosmos

Current `.env.detail` also documents own Command Center Cosmos values, but Generic 0.1.0 does not activate a durable Manager host.

Therefore those variables are not evidence of working durable runtime.

## Distribution

```text
command-center profile
starter                  PASS
wheelhouse                85 packages
portable dependency check PASS
qualification             PRECHECK_PASS
runtime                   UNVERIFIED
```

## Master Projection

PLANNED / REQUIRED, not implemented.

It must become part of the Command Center product runtime/composition, not the Starter.

## Invariants

- Command Center is a separate product from ADA.
- ADA Access/KPI/Collector do not belong to Command Center by symmetry.
- Configuration Manager remains a separate qualification app.
- Product runtime does not move into shared tooling.
- No production identity is inferred from local qualification.
