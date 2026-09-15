# Manager — Source Ledger

Estado: **AUDIT LEDGER**

## Autoridad

- `moragaga/atlanticus:main` = realidad implementada.
- `moragaga/atlanticus-cannonical:main` = autoridad documental vigente.
- `moragaga/atlanticus-decisions` = HISTORICAL.

Git permanece READ ONLY salvo autorización explícita.

## Checkpoint actual

```text
moragaga/atlanticus@384a68fe8fa42263623c95d1d132af2ca54574c8
parent: b2254450b4543d2422ca8580357b9054b515cd6e
```

`atlanticus:main` fue verificado apuntando exactamente a ese commit.

Canonical inspeccionado antes de este reemplazo:

```text
moragaga/atlanticus-cannonical@b58a6c8f0f7631de6789adaee4b913b197be8806
```

## Checkpoints preservados

```text
5fd2858c4bd19c8f9cc416e0996162cb7a3f8c06
    Manager root exact Projection handoff

9342769a626c39d1f7f860f81e051e2ef1300620
    Generic Manager exact-source boundary

567e1a12c862b46dfd7f4ec75c3be750c95bbd54
    Users admin draft baseline semantics

7ffebdbb0b70e41c6f0bd903cc7f27dbd3a05d98
    Users Manager exact-source composition

d23bff025ab899367a8da1178dde5ab50806fe47
    Users admin UI draft cutover

964ec5b3ab4bc9883d1aebf85fbeefbabf657925
    Manager exact Projection boundary

abe6061ffb38c0dde67dfe908fa455c48c387619
    Users exact Projection composition

8ca25e68603f4fb90177dc90f4079b5ab5ff959f
    Users exact Projection host cutover

b2254450b4543d2422ca8580357b9054b515cd6e
    Users exact Projection status cutover

384a68fe8fa42263623c95d1d132af2ca54574c8
    Users exact Source History boundary + host/UI cutover
```

Los commits intermedios del 15-09-2026 entre `d23bff...` y `964ec...` contienen la migración de workspace/source callbacks y exact-only module contract; su realidad final está consolidada en `384a68fe...`.

## Implementación actual inspeccionada

- `web/capabilities/manager`;
- `web/capabilities/source/core`;
- `web/capabilities/projection/core`;
- `web/capabilities/users/configuration`;
- `web/compositions/users-manager`;
- `scopes/ada/web/application/ada-configuration-manager`.

## Estado Users Manager

VERIFIED en main:

- `workflow_service=None`;
- validation exact/canonical;
- exact Source reader;
- exact Source History;
- exact Source publication;
- exact Projection;
- canonical History preview;
- `UsersManagerWorkflowAdapter` eliminado.

## Qualification observada

Reportada por el usuario:

```text
focused Manager + Users Configuration + users-manager  238 passed
full ADA                                               56 passed / 4 failed
```

Los cuatro failures ADA actuales corresponden a adapters legacy Projection no-Users.

No se afirma:

- full Web suite current;
- Docker E2E;
- Python 3.14.7 current;
- CI remoto adicional.

## Conflicto canonical previo a este reemplazo

El canonical anterior todavía afirmaba que:

- el host ADA Users registraba `UsersManagerWorkflowAdapter`;
- productive exact-source Users estaba PLANNED;
- History preview Users seguía legacy;
- exact status/history de Users no estaban cerrados.

Eso contradice `atlanticus:main@384a68fe...`.

Este reemplazo adjudica el conflicto a favor de la realidad implementada y actualiza canonical.

## Decisions repo

`moragaga/atlanticus-decisions` permanece HISTORICAL.

No se utiliza para revertir el lifecycle exacto implementado.

## Siguiente ledger frontier

```text
ADA-LEGACY-PROJECTION-CONTRACT-ALIGNMENT
PLANNED / NEXT
```

Affected:

- Navigation;
- Tools;
- KPI;
- KPI Definitions.

No reabrir Users.
