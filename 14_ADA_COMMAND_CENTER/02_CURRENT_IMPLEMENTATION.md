# ADA Command Center — Current Implementation

Estado: **VERIFIED / UPDATED 2026-09-22**

Corte auditado:

```text
moragaga/atlanticus@7a8c36a29860c8f010c3fe5b840c5f5af4d87d0f
```

## Físicamente en `main`

```text
scopes/ada-command-center/
├── domain/
│   └── alarms/
├── backend/
│   ├── alarms/
│   │   ├── core/
│   │   └── persistence/
│   ├── processes/
│   │   └── alarms-runtime/
│   └── tools/
│       └── catalog/
└── web/
    ├── alarms/
    │   └── configuration/
    └── application/
        └── ada-command-center-configuration-manager/
```

Todavía **no existen**:

```text
scopes/ada-command-center/backend/alarms/materialization
scopes/ada-command-center/backend/processes/alarms-materialization
```

## Clasificación CURRENT

```text
Alarm Domain shared package                         IMPLEMENTED / VERIFIED / CURRENT
Backend Alarm Engine                                IMPLEMENTED / CURRENT
Alarm Configuration Source/Release                  IMPLEMENTED / VERIFIED / CURRENT
Alarm Configuration base Projection                IMPLEMENTED / VERIFIED / CURRENT
Alarm Configuration Manager integration            IMPLEMENTED / VERIFIED / CURRENT
Alarm Configuration Web authoring                   IMPLEMENTED / CURRENT
Command Center Tool Catalog V1                     IMPLEMENTED / VERIFIED / CURRENT
Alarm Tool Reference read model V1                 IMPLEMENTED / CURRENT
B.2 materialization contracts package              NOT YET IMPLEMENTED
Pure B.2 resolver                                   NOT YET IMPLEMENTED
B.2 materialization process                        NOT YET IMPLEMENTED
Runtime/Delivery materialization from B.2           NOT YET IMPLEMENTED
Runtime Effective Head target                       NOT YET IMPLEMENTED
Alarm Live Delivery target                          NOT YET IMPLEMENTED
Management Projection                              NOT YET IMPLEMENTED
```

## Alarm Domain CURRENT

Package:

```text
scopes/ada-command-center/domain/alarms
```

Distribución:

```text
ada-command-center-alarms-domain==1.0.0
```

Namespace:

```python
ada_command_center.domain.alarms
```

Dependencias productivas del package:

```text
[]
```

Authority CURRENT:
- `AlarmIdentity`;
- `AlarmKind`;
- `Criticality`;
- `AlarmDefinition`;
- `MessageDefinition`;
- authoring enums/definitions;
- `AlarmConfiguration`;
- `AlarmConfigurationValidationError`.

Módulos productivos:

```text
models.py
definition.py
configuration.py
errors.py
__init__.py
```

Existe mirror pedagógico equivalente bajo `commented/`.

## Root replacement CLOSED

Las autoridades anteriores ya no existen:

```text
backend/alarms/core/
    src/ada_command_center/alarms/core/definition.py

web/alarms/configuration/
    src/ada_command_center/web/alarms/configuration/models.py
```

No se conservaron re-exports legacy para mantener esos contratos en sus namespaces anteriores.

Backend Alarm Core declara:

```text
ada-command-center-alarms-domain==1.0.0
```

Alarm Runtime declara Domain explícitamente además de Core/Persistence.

Web Alarm Configuration declara Domain y ya no depende de Alarm Core para authoring DTOs.

Configuration Manager declara Domain directamente porque construye/consume `AlarmConfiguration` y authoring definitions.

## Alarm Configuration CURRENT

La unidad editable/publicable sigue siendo:

```text
AlarmConfiguration
├── rules: tuple[AlarmDefinition, ...]
└── messages: tuple[MessageDefinition, ...]
```

La migración de ownership no cambió el contrato durable ni su semántica.

Permanece:

```text
VALID
!=
FULLY RESOLVED
!=
READY
!=
EFFECTIVE
```

Web continúa siendo dueña de:
- Source/Release;
- Projection orchestration;
- Manager workflows;
- Tool reference assistance;
- UI authoring.

El authored contract pertenece al Domain transversal.

## Qualification observada del hito

Ejecutada durante la integración antes del cierre:

```text
Alarm Domain
pytest                    49 passed
ruff check .              All checks passed!
ruff format --check .     12 files already formatted

Backend Alarm Core
pytest                    179 passed
ruff check .              All checks passed!
ruff format --check .     39 files already formatted

Alarms Runtime
pytest                    16 passed
ruff check .              All checks passed!
ruff format --check .     18 files already formatted

Web Alarm Configuration
pytest                    28 passed
ruff check .              All checks passed!
ruff format --check .     26 files already formatted

Configuration Manager
pytest                    11 passed
ruff check .              All checks passed!
ruff format --check .     12 files already formatted
```

Total de tests observados sobre superficies directamente afectadas:

```text
283 passed
```

También pasó:

```text
Alarm Domain extraction static verification
git diff --check
```

No se capturó una segunda corrida completa de esos gates después del commit final `7a8c36a...`.

Por tanto:
- la qualification del árbol integrado previo al commit está **VERIFIED**;
- el checkpoint `main@7a8c36a...` y su estructura final están **VERIFIED**;
- una rerun post-commit exacta permanece **UNVERIFIED** si se exige como gate formal separado.

## Tool Catalog V1 CURRENT

Paquete:

```text
scopes/ada-command-center/backend/tools/catalog
```

Contrato:

```text
ToolCatalogEntry
├── tool_key
├── display_name
├── kind
├── source_release_id
└── structure: ToolStructure

ToolCatalogSnapshot
├── revision
├── generated_at_utc
└── tools
```

`tools` se ordena por `tool_key`; duplicados se rechazan.

`revision` es SHA-256 determinístico sobre el contenido contractual de las entries.

El snapshot se publica sólo después de consolidación completa de todos los inputs requeridos.

## Alarm Tool Reference read model CURRENT

Permanece implementado en:

```text
scopes/ada-command-center/web/alarms/configuration/
src/ada_command_center/web/alarms/configuration/tool_references.py
```

Conserva:
- catalog revision;
- Tool source release;
- components;
- subcomponents;
- `owner_component_key`.

Omite `STRATEGIC` de sugerencias de Alarm Configuration.

No transforma Tool Catalog en autoridad del authored payload.

## Base histórica disponible

Alarm Engine conserva Journey/Evidence/Occurrence y demás hechos operacionales ya auditados.

Alarm Domain Extraction no cambió:
- lifecycle semantics;
- Management suppression;
- Special Condition Runtime reappearance;
- persistence/recovery;
- Analytics boundary.

## Conflicto técnico visible

Project baseline:

```text
Python 3.14.7
```

Packages Command Center CURRENT, incluido el nuevo Domain:

```text
requires-python ==3.14.2
```

Este hito no modificó ese pin.
