# Web Platform — Current Gaps

Estado: **CURRENT / MANAGER COMPOSITION CONVERGENCE NEXT**

Implementation checkpoint:

```text
moragaga/atlanticus@a75465745e188da4765e803595b17acaa55d9306
```

## CLOSED / CURRENT

```text
Manager Core authorization convergence
Users Manager composition
Profiles Manager composition
ADA Generic Manager override semantics
Command Center local Manager override semantics
Command Center Tool Catalog Manager authorization convergence
Command Center Alarm Configuration Manager composition
ADA Starter project-tooling extraction
ADA distribution build/precheck at a7546574
```

## Principal gap transversal

```text
NAVIGATION-MANAGER-COMPOSITION
BLOCKED
```

La composition generic existe pero ADA no la consume.

Findings:

```text
authorization calls obsolete can_access
services registered eagerly in caller registry
generic/ADA workflows differ
generic default source_key != ADA source_key
provider/runtime label contract differs
```

Mientras esto no se cierre, no usar esa composition como si fuera la autoridad final de un segundo producto.

## Manager version authority

CURRENT:

```text
atlanticus-web-manager==0.3.18
owner: web/capabilities/manager/pyproject.toml
```

OPEN:

```text
prove all ADA + Command Center + starter/tooling consumers converge on one authority
```

Si se necesita bump durante el próximo frente, propagar desde el owner.

## Product integration gaps

| Elemento | Estado |
|---|---|
| ADA Generic using common Users/Profiles compositions | CURRENT |
| ADA Generic using common Navigation Manager composition | OPEN / BLOCKED |
| Command Center using common Users/Profiles/Navigation Manager composition | PLANNED |
| Command Center integrated generic host | NOT IMPLEMENTED |
| Tooling/distribution of both consuming same converged Manager | PLANNED / SAME NEXT FRONT |
| Resource Preparation Command Center | PLANNED / AFTER MANAGER CONVERGENCE |
| Azure/Entra productivo | UNVERIFIED |
| Global CI/full monorepo qualification | UNVERIFIED |

## Python / base image

CURRENT package/distribution baseline observed:

```text
Python 3.14.2
```

Historical target:

```text
3.14.7 / python:3.14.7-slim-trixie
```

Migration state:

```text
BLOCKED / DEFERRED UNTIL EXPLICIT USER AUTHORIZATION
```

No usar esta diferencia como NEXT, gate o finding repetitivo en otros incrementos.

## Próximo foco único

```text
MANAGER-COMPOSITION-CONVERGENCE-AND-DUAL-PRODUCT-INTEGRATION
```

Orden interno del mismo frente:

```text
1. converge Manager compositions
2. ADA Generic adoption
3. Command Center adoption
4. tooling/distribution alignment for both
5. qualification
```

No mezclar Resource Preparation, Gunicorn, secretos, Alarm Engine, Live, Analytics ni Python/Trixie migration.
