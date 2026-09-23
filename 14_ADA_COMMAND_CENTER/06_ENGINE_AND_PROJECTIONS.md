# ADA Command Center — Engine and Projections

Estado: **CURRENT ENGINE + SOURCE V3 / OPERATIONAL COSMOS PROJECTION NEXT / B.2 PROCESS LATER**

## Base Alarm Configuration Projection

CURRENT Source:

```text
AlarmConfigurationSnapshot
    configuration
    ToolDependencyManifest(Cn)
```

Base Projection preserves it unchanged and does not reread Tool Catalog.

## Operational projection — NEXT

```text
Alarm Source/Release
-> ProjectionRecord[AlarmConfigurationSnapshot]
-> Cosmos
```

Purpose: durable operational read surface for downstream Materialization.

It is not Runtime Configuration, Delivery Configuration, Live Projection or Management Projection.

## B.2 CURRENT

Pure resolver is implemented.

Its exact Tool input for a published Alarm revision is the persisted `ToolDependencyManifest`, not
latest Tool Catalog.

Other inputs:
- Tool reconciliation qualification;
- Evaluator qualification.

## READY vs EFFECTIVE

```text
B.2 READY != Runtime EFFECTIVE
```

Runtime Adoption later advances Effective Head.

## Exact-key downstream rule

Future Runtime/Delivery/Live align by exact:

```text
AlarmResolutionKey(
    alarm_configuration_revision,
    confirmed_tool_catalog_revision,
)
```

No latest fallback.

## No Tool Cosmos dependency

Materialization/Runtime/Delivery do not access individual Tool Cosmos sources.
Exact Tool topology travels in the Alarm snapshot.

## Order

After Cosmos Projection:
1. Materialization Process;
2. Runtime/Delivery artifact stores;
3. Runtime Adoption/Effective Head;
4. Live Delivery;
5. Management Capture/Projection as separate fronts.
