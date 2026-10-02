# Frontend Generation

Estado: **CURRENT — SHARED ENGINE CLEAN / PRODUCT OWNERSHIP EXPLICIT**

## Shared engine

```text
tooling/distribution/web/
```

owns:

```text
products.toml
generate_starter
build_wheelhouse
qualify_starter
probe_starter
distribute
starter/base
```

It does not own ADA runtime, Command Center runtime or product-specific starter subtrees.

## Generic Starter

The base Starter is a minimal Atlanticus Web application.

It may contain the minimum composition/runtime needed to demonstrate the generic framework contract.

The previous `example` demo module/callback/assets were removed from the base.

## ADA

Product-specific generation support:

```text
scopes/ada/tooling/distribution/web/
```

ADA Starter remains editable host/deployment surface but delegates runtime behavior to `ada-generic-application`.

Project tooling also belongs to the ADA scope.

Master Projection and local resources are **not** Starter code.

## Command Center

Starter:

```text
scopes/ada-command-center/tooling/distribution/web/starter
```

is intentionally minimal and delegates to:

```text
ada-command-center-generic-application
```

## Product catalog

Current profiles:

```text
generic
ada
command-center
```

Shared engine selects strategies/handlers from product catalog instead of hardcoding ADA internals.

## Rule

A product runtime change should normally require a product package change, not a mirrored implementation change in distribution tooling.

Generated artifacts may pin/version the product dependency, but do not become a second runtime authority.
