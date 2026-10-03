# ADA Web — Current Baseline

Estado: **CURRENT — DISTRIBUTED RUNTIME VERIFIED / TOOL CONFIGURATION ROOT CUTOVER OPEN**

## Application

ADA Generic is the product composition root.

Version observed in distributed runtime:

```text
0.2.26
```

## Runtime qualification

From the isolated consumer repository:

```text
Web container              healthy
/health/live               HTTP 200
/health/ready              HTTP 200
environment                local
Cosmos database            visible
Cosmos containers          visible
Cosmos Data Explorer       HTTP 200
```

`/health/ready` still reports `checks: {}`.

## Real configuration evidence

A real Operaciones Integradas Tool Projection and KPI Registry Projection were produced.

The exercise demonstrated that the system can publish/project configuration and exposed the ownership issue before using real Azure Storage.

## UI gaps observed

### Header

Tool display name already belongs to Tool Configuration and is wired to operational branding.

Current worker bootstrap freezes the Tool context; dynamic refresh after Tool reprojection is not yet implemented.

### Time Status

PI and Dispatch are already modeled and labeled in UI contracts.

The runtime timestamp/source feed remains incomplete.

### Navigation

Current `allowed_profiles=[]` means public, so the model cannot express a route available only to privileged `root/local`.

A new explicit public/restricted policy is DECIDED / PLANNED.

## Current priority

Do not redesign UI while the ownership root is wrong.

Next:

```text
Tool-scoped configuration + Users runtime cutover
```

Then return to:

```text
real configuration
recovery
UI operational data
Alarm integration
```
