# ADA Generic — First Deliverable Vertical

Estado: **CURRENT / GENERIC STAGE 1 CLOSED / PRODUCT GOLDEN PATH OPEN**

## Stage 1 — CLOSED

```text
Tool Projection durable
→ resilient Tool resolution
→ ToolStructure
→ KPI Collector when configured
→ Latest / Timeseries
→ process cache
→ browser dcc.Store per ToolComponent
→ consumer handoff
```

No reabrir esta frontera sin finding real.

## Manager / Access — CLOSED en este hito

ADA Generic ya compone autorización Manager sin depender de ADA Access.

Root administrado y trusted-local usan `administrative_override`.
Perfiles ordinarios no reciben administración Manager.

## Golden Path de producto — OPEN

La siguiente qualification de ADA no debe ser otro conjunto de módulos aislados.

Debe existir un recorrido reproducible, construido sólo con contratos reales:

```text
clean runtime/infrastructure
→ resource preparation
→ identity/login
→ Profiles / Users / Navigation as required
→ Tool Configuration
→ publish/materialize Tool Projection
→ ADA runtime consumes Tool
→ KPI configuration/runtime/delivery
→ Latest + Timeseries consumed by ADA
→ Alarm integration where the current alarm contract requires it
→ restart/recovery checks
→ artifact/distribution qualification
```

La secuencia exacta debe confirmarse contra implementación CURRENT antes de ejecutarse.

## Tool consolidation — OPEN / UNVERIFIED

Requisito de producto planteado:

```text
create Tool A
create Tool B that consolidates/consumes A
```

El contrato inspeccionado de `ToolSourceConsumption` sólo demuestra `source_keys`.

No está permitido inventar un `tool_dependency`, un adapter o reutilizar `source_key` como dependencia Tool sin decisión previa.

## Login/data bootstrap — OPEN / UNVERIFIED E2E

La semántica de autorización Manager quedó cualificada.

Permanece por cualificar como producto:

```text
empty durable state
→ resources/data prepared
→ first administrative identity
→ persisted profiles/users/navigation/access
→ subsequent login with expected effective user
```

Esto no autoriza cambios de identidad ni de Users durante el review de Tooling.

## KPI — OPEN E2E, capability existente

Collector y contratos Latest/Timeseries existen.

Pendiente de producto:

```text
definition/registry
→ runtime/backend
→ delivery
→ ADA Collector
→ browser consumption
```

No rediseñar Collector durante Tooling review.

## Alarm — integración separada

ToolStructure ya participa en contratos baseline de Alarm.

El Alarm Engine y Command Center tienen su propio scope.
No duplicar ni rediseñar esos dominios dentro de ADA Generic.

## Próxima frontera única

```text
ADA-TOOLING-CONTRACT-REVIEW
```

Después:

```text
ADA-END-TO-END-GOLDEN-PATH
```
