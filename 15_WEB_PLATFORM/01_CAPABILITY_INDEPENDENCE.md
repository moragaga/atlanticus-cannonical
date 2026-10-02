# Web Platform — Capability Independence

Estado: **CURRENT**

## Rule

Separate:

```text
generic capability
product-specific capability
composition-only integration
```

Do not create product-to-product dependencies for a contract that is genuinely reusable.

## Generic capabilities CURRENT

```text
Source
Projection
Users
Profiles
Navigation
Manager
Master Projection
Storage topology
```

## Product-specific examples

```text
ADA Access
ADA KPI
Command Center Alarm Configuration
Tool-specific product catalogs
```

## Master Projection — RECONCILED

Reusable engine:

```text
web/capabilities/master-projection
```

Product compositions may register different projection domains.

This is valid composition-only integration.

## Source namespace — CURRENT GAP

`SourceStore` is generic.

However Command Center imports:

```text
ada.web.storage.namespace.AdaStorageNamespace
```

That is a product-to-product dependency for a namespace contract reused by both products.

NEXT must either:

```text
extract a genuinely generic namespace capability
```

or prove that the responsibility is product-specific and compose it differently.

Do not solve this with an adapter/alias.

## One owner / one contract

After convergence:

```text
one reusable owner
one public contract
product-specific values supplied by composition
```

No legacy copy remains under ADA merely for compatibility.
