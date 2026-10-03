# ADA Generic — First Tool Delivery Order

Estado: **CURRENT — TOOL CONTRACT BOUNDARY CLOSED / ARTIFACT QUALIFICATION NEXT**

Delivery order remains:

```text
1. Operaciones Integradas
2. Mina
```

## Closed prerequisites relevant to the current release path

Tool-varying configuration has Tool-scoped durable ownership.

Users has its previously established runtime/recovery model.

The additional pre-release ownership boundary is now closed:

```text
ADA Tool transversal contract owner
```

CURRENT owner:

```text
scopes/ada-contracts/tools
ada-contracts-tools==1.0.0
ada.contracts.tools
```

ADA Generic runtime export confirms:

```text
ada-contracts-tools  PRESENT
ada-web-tools        ABSENT
```

## Web Tool Configuration boundary

Tool Configuration remains Web/ADA-specific:

```text
scopes/ada/web/tools/configuration
```

It consumes the transversal contract and owns Web-specific Source/Projection/Branding/persistence/editor responsibilities.

Do not recreate structural/source value objects under `ada.web.tools`.

## Physical legacy retirement

```text
scopes/ada/web/tools/core
```

is **SUPERSEDED** as Web contract owner.

Deletion is **BLOCKED** by Command Center references outside this delivery increment.

Do not make distribution progress depend on cleaning that separate scope once the ADA Generic runtime dependency gate is clean.

## Next prerequisite

Single next focus:

```text
ADA Generic artifact generation qualification
```

Scope:

```text
artifact generation
.env.detail generated contract audit
system-derived vs operator-supplied environment values
generation qualification
```

Do not regenerate the final distributed release until this gate is clean.

## After artifact qualification

```text
distribution regeneration
isolated consumer installation
local Docker runtime
durable Cosmos + Storage configuration
recovery qualification
resume Operaciones Integradas configuration
```

## Later separate ADA flow

```text
KPI runtime / historian / delivery / timeseries
Collector
Time Status
UI
```

Alarm and Command Center cleanup remain separate.
