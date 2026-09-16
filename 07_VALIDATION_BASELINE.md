# Atlanticus — Validation Baseline

Estado: **CURRENT**

## Regla

Qualification y tests son evidencia de propiedades del contrato CURRENT.

No son autoridad para conservar contratos, schemas o adapters SUPERSEDED.

No declarar un cutover CLOSED sólo porque la suite está GREEN.

## Autoridad de implementación

Checkpoint publicado inspeccionado al iniciar este cierre:

```text
moragaga/atlanticus@55cd6121e000a6af5d4f0dc0ea2e384f97a27f2a
```

Existe un working tree local posterior no publicado.

## Hitos

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-MANAGER-GENERIC-CONTRACT-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL
IN PROGRESS

PROJECTION-CORE-STALE-TEST-ALIGNMENT
CLOSED / VERIFIED
```

## Evidencia de Users observada

Ejecutada por el usuario sobre el working tree local:

```text
ruff scoped
All checks passed!

pytest scoped
113 passed

git diff --check
PASS
```

Después de ajustar un test stale de Projection core:

```text
full Web pytest
546 passed
7 skipped
0 failed
```

El ajuste fue sólo de expectativa textual del test:

```text
old expectation:
Projection result source release does not match target

CURRENT production:
Projection result target does not match requested target
```

La producción compara el `ProjectionTarget` completo; no se cambió producción para satisfacer el test antiguo.

## Scan legacy observado

Se ejecutó un scan exacto sobre nombres conocidos del contrato revision-based y dio 0 matches.

Ese scan **no incluía inicialmente la compatibilidad `schema_v1`**, por lo que no era suficiente para declarar clean cutover.

## Hallazgo que invalida el cierre de Users

VERIFIED en el working tree:

```text
schema_v1.py
decode_users_profiles_schema_v1(...)
Source schema-v1 read branch
Projection schema-v1 read branch
```

Adjudicación:

```text
semantic compatibility adapter
SUPERSEDED / REMOVE
```

Que sea read-only o histórico no altera la adjudicación.

## Política de tests refinada

Durante una migración raíz:

```text
DO
- fijar contrato final;
- eliminar legacy;
- eliminar/reemplazar tests del contrato eliminado;
- luego ejecutar tests;
- corregir sólo desalineaciones del contrato final.

DO NOT
- conservar adapters para salvar tests;
- agregar fallback schema viejo para salvar tests;
- adaptar producción al contrato retirado;
- considerar GREEN como criterio suficiente de arquitectura.
```

Si al eliminar legacy la suite rompe, eso es evidencia a adjudicar **después** del cutover.

## Qualification requerida para cerrar Users

Sólo después de eliminar toda compatibilidad schema v1:

```text
ruff scoped
pytest scoped
exact forbidden scan over all web
full Web pytest
git diff --check
```

Criterio de aceptación:

```text
one valid route
zero legacy runtime schemas
zero adapters/shims/aliases
zero revision -> ProjectionTarget reconstruction
zero expected_source_revision
tests validate only CURRENT behavior
```

## Estado global

La suite Web local está GREEN sobre el working tree actual:

```text
546 passed
7 skipped
```

Pero el hito Users permanece `IN PROGRESS` porque el working tree todavía viola una decisión arquitectónica congelada.

## UNVERIFIED

- full Web GREEN después de remover schema v1;
- full ADA suite;
- Docker E2E;
- CI remoto;
- Python 3.14.7/Trixie global;
- Tools/KPI consumers.

## Git

Git continúa SOLO LECTURA para el asistente.
