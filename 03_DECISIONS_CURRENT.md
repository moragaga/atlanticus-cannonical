# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global — FROZEN

```text
uv, no pip normal
contracts before consumers
backend before frontend
clean root cutover
no legacy adapters/shims/aliases
one focus per increment
Git read-only unless explicit authorization
```

Web CURRENT usa Python 3.14.2.
Python 3.14.7/Trixie permanece `PLANNED / DEFERRED`.

## Environment and persistence axes — FROZEN

```text
ATLANTICUS_ENVIRONMENT = local | production
```

controla host/runtime behavior.

Cada producto puede tener un selector de persistencia:

```text
local | durable
```

que controla filesystem/in-process versus stores durables.

No introducir:

```text
storage=local|azure
cosmos=local|azure
```

Emulator y Azure son destinos de conexión.

## Dual-app lockstep — FROZEN

ADA Generic y ADA Command Center Generic participan del mismo checkpoint de madurez para
fronteras compartidas.

Un producto no se declara adelantado en un checkpoint dual hasta que ambos pasen la misma clase
aplicable de validación.

## Master Projection ownership — REFINED / FROZEN

SUPERSEDED:

```text
ADA Generic owns the reusable Master Projection engine
Command Center still needs its own implementation
```

CURRENT:

```text
atlanticus-web-master-projection
    owns reusable engine

ADA Generic
    owns ADA projection-domain composition + provisioning

Command Center Generic
    owns Command Center projection-domain composition + provisioning
```

No duplicar el motor.

## Command Center runtime — REFINED / FROZEN

SUPERSEDED:

```text
Command Center Generic is local-manager-only
```

CURRENT:

```text
local Web host
+
local or durable Manager persistence
```

Production identity remains separate and unimplemented in Generic.

## Source boundary — FROZEN CORE / NEXT CONVERGENCE

`SourceStore` + Local/Blob providers continúan genéricos y frozen.

NEXT no debe reescribir Source Core.

El siguiente incremento debe resolver ownership/composición compartidos por ADA y Command Center,
incluyendo el actual acoplamiento:

```text
ada-command-center -> ada.web.storage.namespace
```

No congelar todavía el nombre/path final de una nueva capability hasta inspeccionar todos los
consumidores reales.

## Tooling ownership — DECIDED DIRECTION / PLANNED

Regla de arquitectura:

```text
/scopes/<owner>/tooling
    product/scope-specific build, distribution and qualification composition

/tooling
    reusable mechanisms and cross-scope orchestration
```

ADA y Command Center ya siguen parcialmente esta regla.

Normalizar Operational Data y futuros backend toolings queda:

```text
PLANNED / DEFERRED
```

No es blocker para levantar las aplicaciones.

## Próximo orden — FROZEN

```text
1. Source namespace/composition convergence
2. lift ADA Generic + Command Center Generic
   using LOCAL HOST + DURABLE PERSISTENCE
3. validate real durable runtime / Master Projection
4. then continue ADA KPI/Collector/UI
5. tooling topology normalization later
```
