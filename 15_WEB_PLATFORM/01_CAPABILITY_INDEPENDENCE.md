# Web Platform — Capability Independence

Estado: **CURRENT — ADA TOOL CONTRACT OWNER RECONCILED**

## Rule

Separate:

```text
generic capability
product-specific capability
transversal product contract
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

## ADA transversal contracts CURRENT

Cuando un contrato pertenece a ADA pero es compartido por más de un producto ADA, su owner no debe quedar dentro de un producto Web específico.

Tool contracts CURRENT:

```text
owner:     scopes/ada-contracts/tools
package:   ada-contracts-tools==1.0.0
namespace: ada.contracts.tools
```

Consumidores pueden incluir Web, Command Center o backend cuando intercambian exactamente ese contrato.

Las responsabilidades Web específicas siguen en sus packages Web.

Ejemplo:

```text
ada.contracts.tools
        |
        +--> ada.web.tools.configuration
                 |
                 +--> Source / Projection
                 +--> Branding
                 +--> persistence composition
                 +--> editor / callbacks / presentation
```

No mover a `ada-contracts` servicios, persistencia o UI sólo porque usan los value objects compartidos.

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

Convergence rule:

```text
one reusable owner
one public contract
product-specific values supplied by composition
```

For Tool structural/source contracts this convergence is complete in the ADA Generic release-chain:

```text
ada-contracts-tools  PRESENT
ada-web-tools        ABSENT
```

The old physical directory:

```text
scopes/ada/web/tools/core
```

is **SUPERSEDED as owner** but its deletion is **BLOCKED** by out-of-scope Command Center references.

This temporary physical presence must not be interpreted as permission to add new consumers.

No legacy copy remains as an accepted compatibility contract.
