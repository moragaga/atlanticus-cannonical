# Atlanticus Canonical Baseline 1.0 — Closure

Fecha: **2026-09-12**

## Status

```text
BOOTSTRAP ARCHITECTURAL DESIGN
→ CLOSED FOR EXECUTION
```

No significa que todo esté implementado.

Significa que existe suficiente autoridad y dirección para avanzar sin seguir ampliando arquitectura transversal.

## Primera secuencia de producto

```text
1. Operaciones Integradas
2. Mina
```

## Vertical de trabajo

Cada incremento debe cerrar una capacidad de punta a punta:

```text
contract/backend
→ source/projection
→ domain/runtime
→ Web consumer
→ artifact/distribution
→ validation
```

## Web deployment

```text
base infrastructure
→ Web
→ resource/projection readiness
→ Backend
```

La Web puede existir sin datos/backend.

## Projection model

```text
independent base projections
→ derived resolutions only when real dependencies exist
```

No existe un orden global obligatorio de todas las proyecciones.

## Manager

- header propio;
- Login/Bootstrap Console real;
- no local admin bypass;
- histories + projection states;
- external Component links via metadata + JS popover + warmup.

## User Activity

```text
1 user + 1 session + 1 page = 1 document
```

- return to same page increments counters;
- reload preserves session;
- page change creates another page document;
- watcher five minutes / visibility behavior preserved;
- TTL 24 h;
- partition key remains open until workload review.

## KPI backend

Common per-job:

```text
REPROCESS_CURRENT=false
```

No global flag.

## Command Center

Analytics is deferred.

First prove that Engine/History exposes all required facts.

## Productization still required

- containers/resource inventory;
- SourceStore/Blob;
- generators;
- distribution packages;
- scripts/master gate;
- env.detail;
- READMEs;
- loaders;
- alarm management Web;
- University cases.

These are execution items, not reasons to reopen the architecture bootstrap.

## DevOps boundary

Atlanticus delivers distribution-ready artifacts.

External DevOps owns corporate pipeline implementation.

## Preservation

Original decision/qualification sources remain retained.

Canonical Markdown is a navigation/current-state layer, not a replacement for original evidence.
