# Alarm Engine — Decision Index

Estado: **CURRENT / HISTORICAL SOURCES + IMPLEMENTATION REFINEMENTS**

| ID | Tema | Estado |
|---|---|---|
| ALARM-DEF-B1 | Historical Alarm Definition base | HISTORICAL / FROZEN INPUT |
| ALARM-PROJ-B2 | Historical projection/publication decisions | HISTORICAL / REFINED |
| ALARM-B2-PURE-RESOLVER | Pure deterministic resolver | CURRENT / IMPLEMENTED |
| ALARM-TOOL-MANIFEST | Exact Tool evidence frozen with Alarm source | CURRENT / IMPLEMENTED |
| ALARM-SOURCE-V3 | AlarmConfigurationSnapshot + ToolDependencyManifest | CURRENT / IMPLEMENTED |
| ALARM-TOOLS-FREEZE | validate/publish Cn correlation | CURRENT / IMPLEMENTED |
| ALARM-RANK-SUPPRESSION | priority_order suppression | CURRENT |
| ALARM-DEACTIVATION-CASCADE | active deactivation sustains cascade | CURRENT |
| ALARM-RUNTIME-VISIBILITY | no delivery_enabled / no SHADOW | CURRENT |

## Historical decisions

```text
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

Se preserva:
- Live vs Management separation;
- one coherent resolution -> Runtime + Delivery;
- `INVALID != REMOVED`;
- `READY != EFFECTIVE`;
- backend priority authority.

## Refinements CURRENT

### Physical authority

SharePoint wording histórico no es CURRENT para dominios migrados.
Storage/Blob es durable target authority.

### Tool topology

```text
Confirmed Tool Catalog -> Command Center Cosmos
```

queda SUPERSEDED.

CURRENT:

```text
upstream Tool projections
+ Storage prior state
-> reconciliation/certification
-> Confirmed Tool Catalog
-> Storage
-> END
```

### Alarm/Tool correlation

Historical:

```text
same Alarm revision + later Tool revision
```

queda SUPERSEDED.

CURRENT:

```text
Alarm release Rn
-> ToolDependencyManifest(Cn)
```

### Save validity

```text
LATEST SAVED = LATEST VALID_AT_SAVE
VALID_AT_SAVE != B.2 READY != EFFECTIVE
```

CURRENT publication valida intrinsic Alarm + Tool revision/existence correlation.
Evaluator qualification y otras checks B.2 son posteriores.

### History

`display_name` + `ToolStructure` se congelan para que historia no dependa del contrato Tool actual.

## Authority

```text
atlanticus:main CURRENT
> atlanticus-cannonical CURRENT
> atlanticus-decisions history
```
