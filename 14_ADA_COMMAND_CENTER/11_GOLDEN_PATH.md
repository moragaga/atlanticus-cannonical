# ADA Command Center — Golden Path

Estado: **PARTIALLY IMPLEMENTED / ALARM SNAPSHOT V3 CLOSED / COSMOS PROJECTION NEXT**

## Current path

```text
1. Tool owners publish Tool projections
2. Command Center reconciliation/certification
3. Confirmed Tool Catalog Cn -> Storage
4. Alarm authoring consumes one current Tool snapshot
5. Save Draft pins Cn
6. Validate aggregate + Tool correlation/existence
7. Publish Alarm Source release Rn with ToolDependencyManifest(Cn)
8. Base Projection preserves exact snapshot
9. Operational Alarm Configuration Projection -> Cosmos       NEXT
10. B.2 Materialization Process                               PLANNED
11. Runtime + Delivery artifacts READY                        PLANNED
12. Runtime Adoption -> EFFECTIVE                             PLANNED
13. Live Delivery                                             PLANNED
```

## Existing Alarm revision

```text
R1/C1
Tools -> C2
```

does not silently become `R1/C2`.

If the user does nothing, `R1/C1` remains published.

A new Alarm publication may adopt C2 after Save Draft/Validate/Publish.

## Manifest guarantees

A published Alarm revision carries:
- exact Tool catalog revision;
- every referenced Tool;
- Tool source release lineage;
- Tool display name;
- full ToolStructure.

No historical Tool lookup is required for B.2.

## Current Definition of Done

```text
Tool Catalog V1                         DONE
Structured Alarm authoring             DONE
domain/tools                            DONE
ToolDependencyManifest                 DONE
Source schema v3                       DONE
validate/publish drift protection      DONE
Pure B.2 resolver                      DONE

Alarm Config Cosmos Projection         NEXT
Materialization Process                AFTER
Runtime Adoption                       LATER
Live Delivery                          LATER
```
