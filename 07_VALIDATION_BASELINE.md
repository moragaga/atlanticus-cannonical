# Atlanticus — Validation Baseline

Estado: **CURRENT**

## Regla

Qualification y tests son evidencia de propiedades del contrato CURRENT.

No son autoridad para conservar contratos, schemas o adapters SUPERSEDED.

No declarar un cutover CLOSED sólo porque la suite está GREEN.

## Autoridad de implementación

```text
moragaga/atlanticus@a065f45c55a527c96ce333705465487e95f0a737
```

## Hitos

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-MANAGER-GENERIC-CONTRACT-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-CLEAN-CUTOVER-COMPLETION
CLOSED / VERIFIED / CURRENT

USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL
CLOSED / VERIFIED / CURRENT

PROJECTION-CORE-STALE-TEST-ALIGNMENT
CLOSED / VERIFIED
```

## Evidencia final de Users

```text
ruff scoped
All checks passed!

pytest scoped
99 passed

full Web pytest
545 passed
7 skipped
0 failed

git diff --check HEAD^..HEAD
PASS

git status --short
CLEAN
```

## Forbidden scan

Lista de control:

```text
schema_v1
decode_users_profiles_schema_v1
expected_source_revision
UsersConfigurationCatalog
UserProfileConfiguration
split_legacy_users_configuration_catalog
```

Resultado sobre código CURRENT:

```text
PASS / zero matches
```

Los matches observados inicialmente para `expected_source_revision` estaban sólo bajo `build/` generado de Manager y no pertenecían al source CURRENT.

## Clean cutover implementado

Removido:

```text
schema_v1.py
decode_users_profiles_schema_v1(...)
Source schema-v1 read branch
Projection schema-v1 read branch
tests cuyo propósito era preservar lectura schema v1
```

Los tests CURRENT rechazan versiones no vigentes donde corresponde.

## Política de tests vigente

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

## Criterio de aceptación de Users

```text
one valid route
zero legacy runtime schemas
zero adapters/shims/aliases
zero revision -> ProjectionTarget reconstruction
zero expected_source_revision
tests validate only CURRENT behavior
```

Resultado:

```text
ACCEPTED
CLOSED / VERIFIED / CURRENT
```

## UNVERIFIED

- full ADA suite;
- Docker E2E;
- CI remoto;
- Python 3.14.7/Trixie global;
- Tools consumer;
- KPI Configuration consumer;
- KPI Definition consumer.

## Git

Git continúa SOLO LECTURA para el asistente.
