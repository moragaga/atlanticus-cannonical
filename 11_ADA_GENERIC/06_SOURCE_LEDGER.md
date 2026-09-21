# ADA Generic — Source Ledger

Estado: **AUDIT LEDGER**

## Autoridad vigente

```text
Implementation
moragaga/atlanticus@21cfb2f11362c1606ad14ff8adc7551948eced6a

Parent
6155dae407dc784114ff34c7b3b6f93125432713

Tree
48e4115a5fb53e64d83e2ae2243a9f11d612d26f

Canonical inspected before replacement
moragaga/atlanticus-cannonical@4058aab3525a09b568b80f3f6a5265e45e4f6fea

Historical decisions
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

## Checkpoints de este hito

```text
366e2bab5bd4de6cdf94773b4fc4be5d5b1f26c1
Tool Source -> Tool Projection -> Collector factory baseline

6155dae407dc784114ff34c7b3b6f93125432713
AdaStorageNamespace
Tool Projection document codec
LocalToolProjectionStore
CosmosToolProjectionStore

21cfb2f11362c1606ad14ff8adc7551948eced6a
ToolPersistenceSettings
ToolPersistenceComposition
resolve_active_tool_projection
project_current_tool_source
```

## Sources CURRENT inspeccionadas

```text
scopes/ada/web/storage/namespace/
scopes/ada/web/tools/configuration/
scopes/ada/web/tools/projection-local/
scopes/ada/web/tools/projection-cosmos/
scopes/ada/web/tools/persistence/
scopes/ada/web/application/ada-generic-application/
web/capabilities/source/local/
web/capabilities/source/blob/
web/capabilities/projection/core/
connectivity/storage/
connectivity/cosmos/
```

## Qualification observada

```text
storage namespace
15 passed
ruff check PASS
ruff format --check PASS
git diff --check PASS

Tool Projection persistence
configuration codec 2 passed
projection-local    3 passed
projection-cosmos   5 passed
ruff check PASS

Tool persistence composition
10 passed
ruff check PASS
ruff format --check PASS
git diff --check PASS
```

El rerun completo de Tool Projection persistence posterior al último format antes de
`6155dae...` no fue mostrado; permanece `UNVERIFIED` como qualification final exacta, aunque la
implementación publicada es CURRENT.

## Canonical conflict before replacement

Canonical `4058aab3525a09b568b80f3f6a5265e45e4f6fea` todavía describe como siguiente paso:

```text
ADA-GENERIC-COLLECTOR-OPERATIONAL-INTEGRATION
```

pero implementación CURRENT ya añadió un prerrequisito arquitectónico/persistente que ese texto
no refleja:

```text
namespace
Tool Projection durable
provider composition
resilient Tool resolution
```

Clasificación:

```text
IMPLEMENTATION CURRENT
CANONICAL STALE
REPLACEMENT REQUIRED
```

## Gap real CURRENT

ADA Generic todavía usa:

```text
_StartupToolProjectionStore
resolve_current_tool_projection
```

con fallo estricto ante ausencia de Source current.

Siguiente frontera:

```text
ADA-GENERIC-OPERATIONAL-BOOTSTRAP
PLANNED / NEXT
```
