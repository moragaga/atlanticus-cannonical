# Web Platform — Resource Provisioning

Estado: **CURRENT — ADA GENERIC PREPARATION REUSED BY SPECIALIZED INTEGRATED OPERATIONS PRODUCT**

## Principle

Resource preparation belongs to deployment/application composition, not to every backend job and not to Web worker startup.

A capability declares physical requirements; product composition decides which resources are required for that application.

Runtime startup consumes prepared resources.

## ADA Generic CURRENT

ADA Generic has explicit durable resource preparation through:

```text
ada.web.application.generic.manager_deployment:manager_resources_main
```

Actions:

```text
prepare
validate
```

The preparation plan resolves the durable Blob/Cosmos topology required by Generic/Manager capabilities.

This remains the authoritative implementation.

## Specialized ADA products CURRENT

A specialized product may expose a product-named CLI while delegating to Generic preparation.

It must not copy the resource topology or provisioning implementation.

Current example:

```text
ada-integrated-operations-resources
    ↓
ada.web.application.generic.manager_deployment:manager_resources_main
```

This means future Generic durable topology changes are inherited by the specialized product without a parallel list.

## Integrated Operations local infrastructure CURRENT

The product owns a local deployment harness:

```text
scopes/ada/web/application/ada-integrated-operations-application/
└── deployment/local/compose.yaml
```

It defines where local emulators live:

```text
Azurite
Cosmos Emulator
published host ports
```

It does not redefine what application resources Generic requires.

Separation:

```text
deployment/local
    → WHERE local infrastructure runs

Generic resource preparation
    → WHAT durable resources are required

Integrated Operations product command
    → product-facing delegation

Web startup
    → consume existing resources
```

## Current local sequence

```text
docker compose -f deployment/local/compose.yaml up -d
uv run ada-integrated-operations-resources prepare
uv run ada-integrated-operations-resources validate
uv run ada-integrated-operations-application
```

Do not fold `prepare` into every Flask/Gunicorn worker startup.

## Command Center

Durable runtime resolves and uses:

```text
Storage container
Command Center Cosmos database
alarm-configuration
profiles-projection
navigation-projection
users-runtime
```

The previously documented Command Center parity status remains independent from this Integrated Operations closure.

No Command Center provisioning change is claimed here.

## Namespace interaction

Resource preparation must consume the same Source/namespace contract selected by the application.

Do not encode a separate product-specific durable path scheme in provisioning.

## Qualification state

Verified from implementation:

```text
Integrated Operations compose harness exists
product resource entry point delegates directly to Generic
Web startup remains separate
```

Exact specialized `prepare` / `validate` command output was not captured in this closure.

Classification:

```text
UNVERIFIED runtime evidence
```

## Separate

Azure permissions, production identity, destructive migration and production infrastructure remain separate.
