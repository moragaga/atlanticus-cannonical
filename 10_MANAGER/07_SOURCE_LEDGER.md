# Manager — Source Ledger

Estado: **AUDIT LEDGER**

## Autoridad

- `moragaga/atlanticus:main` = realidad implementada publicada.
- `moragaga/atlanticus-cannonical:main` = autoridad documental vigente.
- `moragaga/atlanticus-decisions` = HISTORICAL.
- Git permanece SOLO LECTURA para el asistente.

## Checkpoint publicado de este cierre

```text
moragaga/atlanticus@27c2e4beed125fe379881048f0df5fbe3ff6cb1a
```

Parent inmediato:

```text
a065f45c55a527c96ce333705465487e95f0a737
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

## Navigation

```text
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## Users

```text
USERS-MANAGER-GENERIC-CONTRACT-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-CLEAN-CUTOVER-COMPLETION
CLOSED / VERIFIED / CURRENT

USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL
CLOSED / VERIFIED / CURRENT
```

## Tools Source/Projection

Publicado en `27c2e4beed125fe379881048f0df5fbe3ff6cb1a`.

```text
TOOLS-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Agregado:

```text
source_release.py
source_projection.py
ToolSourceService
ToolProjectionBuilder
create_tool_projection_service(...)
```

Consume directamente:

```text
atlanticus.web.source
atlanticus.web.projection
```

Removido del paquete Tools Configuration CURRENT:

```text
contracts.py
lifecycle.py
projection.py
services.py
source.py
ToolLifecycle*
private Source snapshot/revision contract
private Projection snapshot/revision contract
```

## Desalineación temporal conocida

`ada-configuration-manager` todavía contiene:

```text
ToolLifecycleServices import
ToolConfigurationManagerWorkflowAdapter
expected_source_revision
```

No es un motivo para reintroducir legacy en Tools.

Se resolverá en el cutover final del consumer.

## Qualification

Para Tools:

```text
CURRENT tests added
VERIFIED

scoped execution result
UNVERIFIED

CI remote status
UNVERIFIED
```

No atribuir a este checkpoint los resultados de qualification global del cierre Users.

## Decisión refinada

La transición entre dominios no requiere que `ada-configuration-manager` permanezca ejecutable.

```text
DOMAIN FINAL CONTRACT FIRST
CONSUMER CUTOVER LAST
NO TEMPORARY COMPATIBILITY
```

## Historical decisions

`atlanticus-decisions` continúa HISTORICAL.

La regla histórica inspeccionada de Manager mantiene que el workflow es genérico y la configuración concreta pertenece al dominio. Eso es consistente con Tools CURRENT.

## Próxima frontera

```text
KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
PLANNED / NEXT
```

No mezclar Manager final ni KPI Definition.
