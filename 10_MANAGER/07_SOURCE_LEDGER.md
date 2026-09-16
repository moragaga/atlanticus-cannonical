# Manager — Source Ledger

Estado: **AUDIT LEDGER**

## Autoridad

- `moragaga/atlanticus:main` = realidad implementada publicada.
- `moragaga/atlanticus-cannonical:main` = autoridad documental vigente.
- `moragaga/atlanticus-decisions` = HISTORICAL.
- Git permanece SOLO LECTURA para el asistente.

## Checkpoint publicado de este cierre

```text
moragaga/atlanticus@a065f45c55a527c96ce333705465487e95f0a737
```

Parent inmediato:

```text
ec9bd35455b8221180b3f15740b58e34766f6112
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

## Navigation

```text
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Sin doble contrato ni adapters de transición.

## Users Manager

```text
USERS-MANAGER-GENERIC-CONTRACT-CUTOVER
CLOSED / VERIFIED / CURRENT
```

No deben reaparecer:

```text
ExactSource*
ExactProjection*
expected_source_revision
revision -> ProjectionTarget reconstruction
```

## Users Configuration clean cutover

Publicado en `a065f45c55a527c96ce333705465487e95f0a737`.

Removido:

```text
schema_v1.py
decode_users_profiles_schema_v1(...)
Source schema-v1 fallback
Projection schema-v1 fallback
```

Resultado:

```text
USERS-CLEAN-CUTOVER-COMPLETION
CLOSED / VERIFIED / CURRENT

USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL
CLOSED / VERIFIED / CURRENT
```

## Qualification de cierre

```text
ruff scoped
PASS

pytest scoped
99 passed

forbidden scan sobre código CURRENT
PASS / zero matches

full Web pytest
545 passed
7 skipped
0 failed

git diff --check HEAD^..HEAD
PASS

git status --short
CLEAN
```

## Decisión refinada

El clean cutover no admite excepciones para compatibilidad histórica permanente.

```text
OLD SCHEMA READERS IN CURRENT RUNTIME
FORBIDDEN
```

Si existe migración real de datos persistidos, debe ser una operación explícita separada y respaldada por evidencia del entorno.

## Projection core stale test

```text
PROJECTION-CORE-STALE-TEST-ALIGNMENT
CLOSED / VERIFIED
```

Producción CURRENT valida:

```text
projection.target == requested target
```

No se reabre desde este cierre.

## Conflictos documentales resueltos por este reemplazo

El canonical anterior todavía describía:

```text
Users clean cutover PLANNED / NEXT
Users legacy contract removal IN PROGRESS
schema v1 compatibility PRESENT
```

Eso quedó desactualizado frente a `atlanticus:main@a065f45c...` y la qualification final.

## Historical decisions

`atlanticus-decisions` continúa HISTORICAL.

No puede reintroducir schemas/adapters legacy ni reemplazar el contrato CURRENT.

## Próxima frontera

```text
TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED / NEXT
```

No mezclar KPI.
