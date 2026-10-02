# ADA Command Center — Current Implementation

Estado: **CURRENT — PRODUCT HOST + PORTABLE DISTRIBUTION AVAILABLE**

## Generic Application

```text
ada-command-center-generic-application==0.1.0
```

Role:

```text
real Command Center Web product composition root
```

Composition includes:

```text
Home
Identity
Projected Navigation
Navigation authorization
Manager surface/modules
Command Center pages
Manager pages
```

## Runtime CURRENT

Launcher uses:

```text
ManagerConfigurationReader
open_local_application(...)
LocalIdentityProvider
local Manager provider
```

Production or durable Manager host is not implemented in Generic 0.1.0.

Current local Manager state includes in-process Users/Profiles/Navigation projections/administration where already documented.

Tool Catalog uses Storage even under local Manager provider.

## Configuration Manager relationship

```text
ada-command-center-configuration-manager==0.1.2
```

remains a separate development/testing/qualification application.

Generic reuses its contracts/composition; it does not execute it as a nested service.

## Distribution CURRENT

Starter ownership:

```text
scopes/ada-command-center/tooling/distribution/web/starter
```

The Starter is intentionally thin and delegates to the real Generic Application.

Qualification:

```text
starter             PASS
wheelhouse packages 85
dependency_check    PASS
status              PRECHECK_PASS
runtime             UNVERIFIED
```

## Master Projection

Requirement:

```text
Command Center needs Master Projection
```

Implementation:

```text
NOT IMPLEMENTED
```

Do not place it in distribution tooling and do not depend on ADA Generic to obtain it.

## Next

Audit/freeze `.env.detail` and identify the smallest runtime/configuration change required for:

```text
Storage final
Cosmos local
Master Projection
```
