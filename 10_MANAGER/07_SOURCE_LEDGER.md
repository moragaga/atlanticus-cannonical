# Manager — Source Ledger

Estado: **AUDIT LEDGER**

## Autoridad

- `moragaga/atlanticus:main` = realidad implementada publicada.
- `moragaga/atlanticus-cannonical:main` = autoridad documental vigente.
- `moragaga/atlanticus-decisions` = HISTORICAL.
- Git permanece SOLO LECTURA para el asistente.

## Checkpoint publicado de este cierre

```text
moragaga/atlanticus@55cd6121e000a6af5d4f0dc0ea2e384f97a27f2a
```

Existe un working tree local posterior con cambios no publicados.

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

Durante este cierre se verificó y corrigió la composición `web/compositions/users-manager`.

Resultado conceptual:

```text
USERS-MANAGER-GENERIC-CONTRACT-CUTOVER
CLOSED / VERIFIED / CURRENT
```

No deben reaparecer:

```text
ExactSource*
ExactProjection*
expected_source_revision
```

## Users Configuration working tree

Se ejecutó un cutover amplio en el working tree local:

- remoción de services/contracts/bundle/projection revision-based;
- remoción de adapters de configuración antiguos;
- remoción de modelos `UsersConfigurationCatalog` / `UserProfileConfiguration`;
- alineación a `UsersProfilesConfiguration`;
- alineación de runtime projection al `ProjectionRecord` genérico;
- remoción de revision→target.

Qualification observada:

```text
ruff scoped: PASS
pytest scoped: 113 passed
git diff --check: PASS
full Web pytest: 546 passed, 7 skipped
```

## Error de implementación detectado antes del cierre

El cutover introdujo:

```text
schema_v1.py
decode_users_profiles_schema_v1(...)
Source schema-v1 fallback
Projection schema-v1 fallback
```

Adjudicación:

```text
SUPERSEDED / REMOVE
```

Razón:

su única responsabilidad es comprender el schema anterior.

Eso es un adapter de compatibilidad semántico aunque no use la palabra Adapter.

## Decisión refinada

El clean cutover no admite excepciones para compatibilidad histórica permanente.

```text
OLD SCHEMA READERS IN CURRENT RUNTIME
FORBIDDEN
```

Si existe migración real de datos persistidos, debe ser una operación explícita separada.

## Projection core stale test

La full suite había quedado con un único test que esperaba un mensaje anterior.

Producción CURRENT valida:

```text
projection.target == requested target
```

El test fue alineado con el contrato actual sin cambiar producción.

Resultado posterior:

```text
546 passed
7 skipped
```

## Conflictos documentales

El canonical anterior todavía decía:

```text
Users Manager alignment PLANNED / NEXT
global qualification BLOCKED during collection
```

Eso está desactualizado respecto de la evidencia de este cierre.

A la vez, el working tree local de Users no puede declararse CURRENT porque:

- no está publicado;
- todavía contiene compatibilidad schema v1 prohibida.

## Historical decisions

`atlanticus-decisions` no fue re-auditado exhaustivamente durante este cierre.

No se le concede autoridad para reintroducir schemas/adapters legacy.

## Próxima frontera

```text
USERS-CLEAN-CUTOVER-COMPLETION
PLANNED / NEXT
```

No mezclar Tools/KPI.
