# ADA Command Center — Open Items

Estado: **OPEN / ALARM TOOL MANIFEST V3 CLOSED / COSMOS PROJECTION NEXT**

Checkpoint:

```text
moragaga/atlanticus@880cb692054c2cd78cdc29cf62ab7b16bbd2c3d6
```

## CLOSED

- Alarm Domain extraction.
- Tool Catalog V1.
- structured Alarm Configuration authoring.
- Pure B.2 resolver.
- Command Center `domain/tools`.
- ToolDependencyManifest.
- historical Tool name/structure capture.
- AlarmConfigurationSnapshot v3.
- Source schema v3.
- workspace Tool revision pin.
- validate/publish Tool drift protection.
- exact Alarm release -> Tool revision correlation.

## NEXT — single focus

```text
ALARM-CONFIGURATION-OPERATIONAL-COSMOS-PROJECTION
```

Need to close:
- ProjectionStore implementation for `AlarmConfigurationSnapshot`;
- Cosmos document contract;
- database/container/partition/id;
- named connection/settings ownership;
- write/read semantics;
- exact Source release provenance;
- failure behavior;
- composition.

Do not mix Materialization Process.

## AFTER

### B.2 Materialization Process

Consumes operational Alarm Projection and embedded Tool manifest.

### Tool qualification producer

OPEN.

### Evaluator qualification producer

OPEN.

### Runtime/Delivery/findings stores

OPEN.

### Runtime Adoption / Effective Head

PLANNED.

### Live Delivery

PLANNED.

### Management Capture / Management Projection

PLANNED / separate.

### UI notification of newer Tool revision

Explicit UX for saved C1 vs current C2 remains OPEN.

### Domain/catalog normalization

DEFERRED.

### Source v2 existing data

UNVERIFIED in target deployments.
No legacy reader exists.

### Python metadata

```text
3.14.7 target
3.14.2 current Command Center metadata
```

OPEN / SEPARATE.
