# Manager — Source Ledger

Estado: **AUDIT LEDGER**

## Autoridad

- `moragaga/atlanticus:main` = realidad implementada.
- `moragaga/atlanticus-cannonical:main` = autoridad documental vigente.
- `moragaga/atlanticus-decisions` = HISTORICAL.

Git permanece READ ONLY salvo autorización explícita.

## Checkpoint actual

```text
moragaga/atlanticus@d34cda3838a67907728b382e238f0178f9f1a64e
parent: 59fcd3ecc8f3441e64fbe0fc892b4467fa56f181
```

Canonical inspeccionado antes de este reemplazo:

```text
moragaga/atlanticus-cannonical@dc7cbe626148c1b82cb2219c52cd99efafddc9d4
```

## Manager core

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Contrato:

```text
ManagerModule
source_service
source_reader_service
projection_service
draft_validation_service
source_history_service | None
```

Source:

```text
SourceReaderWorkflow
SourcePublicationWorkflow
SourceHistoryWorkflow
```

Projection:

```text
ProjectionStatus
ProjectionTarget
ProjectionExecutionResult
```

Workspace:

```text
ManagerWorkspace schema 2
BASE = SourceSnapshot
```

## Navigation change implemented

```text
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Commit actual:

```text
d34cda3838a67907728b382e238f0178f9f1a64e
```

Diff reportado para el cierre:

```text
84 files changed
2702 insertions
4428 deletions
```

El cambio:

- elimina configuración/adapters legacy de Navigation;
- crea `projection-local`;
- crea `projection-cosmos`;
- crea `navigation-manager`;
- actualiza `navigation-configuration`;
- actualiza workspace/lock.

## Qualification observada

```text
Python 3.14.2
ruff scoped: PASS
pytest scoped: 102 passed
forbidden legacy scan: 0 results
git diff --check: PASS
git diff --cached --check: PASS
```

## Full suite finding

`uv run pytest` global quedó bloqueado durante collection.

Cuatro errores visibles nacieron al importar `web/compositions/users-manager`, inicialmente por `ExactSourceHistoryReadResult`.

Inspección de `atlanticus:main` confirma que `users-manager` conserva:

```text
exact_history.py
exact_source.py
exact_projection.py
workspace.py
```

y referencias `Exact*` incompatibles con el Manager CURRENT.

## Causalidad

VERIFIED:

- el commit Navigation tiene parent `59fcd3e...`;
- `users-manager` no forma parte del cambio Navigation;
- la desalineación Users es preexistente;
- Navigation no causó el bloqueo.

## Canonical conflict de este cierre

Antes de este reemplazo, canonical todavía marcaba:

```text
Navigation PLANNED / NEXT
checkpoint 59fcd3e...
```

Eso quedó desactualizado respecto de `atlanticus:main@d34cda3...`.

Este reemplazo debe adjudicar el conflicto a favor de la implementación actual y marcar Navigation CLOSED/CURRENT.

## Historical decisions

`atlanticus-decisions` permanece HISTORICAL.

No se realizó en este cierre una auditoría exhaustiva nueva del repositorio histórico.

Por tanto:

- no se identifica un nuevo conflicto histórico adicional como VERIFIED;
- cualquier conflicto no documentado previamente permanece UNVERIFIED;
- ninguna decisión histórica puede reintroducir contratos `Exact*` en Manager por encima de `atlanticus:main` + canonical CURRENT.

## Próxima frontera

```text
USERS-MANAGER-ALIGNMENT-VALIDATION
PLANNED / NEXT
```

Objetivo:

- validar, no implementar;
- usar obligatoriamente `atlanticus:main`;
- usar obligatoriamente `atlanticus-cannonical:main`;
- no reabrir Navigation;
- no mezclar Tools/KPI.
