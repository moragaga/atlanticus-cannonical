# Atlanticus — Open Questions

Estado: **CURRENT — SOURCE CONVERGENCE NEXT**

## CLOSED

```text
dual .env.detail alignment
Command Center durable Manager composition
shared Master Projection extraction
ADA Master Projection adoption
Command Center Master Projection adoption
```

No reabrir sin finding real.

## OPEN / NEXT — Source namespace and composition

VERIFIED implementation gap:

```text
ada-command-center
    imports
ada.web.storage.namespace.AdaStorageNamespace
```

Preguntas a resolver en el siguiente incremento:

```text
1. ¿Cuál es la responsabilidad reusable mínima del namespace?
2. ¿Debe vivir como capability Atlanticus Storage/Source o integrarse en topology existente?
3. ¿Cómo representar application namespace + optional sub-scope sin llamar "tool" a Command Center?
4. ¿Qué rutas/prefixes deben seguir siendo product-specific?
5. ¿Qué consumidores actuales dependen de AdaStorageNamespace?
6. ¿Puede eliminarse la asimetría location/settings sin crear otra capa innecesaria?
```

Frozen:

```text
SourceStore Core
Source Local
Source Blob
release/concurrency/integrity semantics
```

No modificar esos contratos sin finding específico.

## OPEN — dual application runtime smoke

Después de Source convergence:

```text
ADA Generic + Command Center Generic
LOCAL HOST + DURABLE PERSISTENCE
```

UNVERIFIED:

```text
real current-head Storage/Cosmos connectivity
Command Center explicit resource preparation
Master material end-to-end in both apps
runtime restart/recovery
```

## OPEN / DEFERRED — tooling topology

Dirección acordada:

```text
scope-specific tooling -> /scopes/<scope>/tooling
cross-scope orchestration -> /tooling
```

Pendiente futuro:

```text
Operational Data scope tooling normalization
ADA backend distribution tooling
Command Center backend distribution tooling
```

No abrir ahora.

## Separate

```text
production Entra/Azure
Python 3.14.7/Trixie
current-head artifact regeneration/qualification
KPI/Collector/UI
```
