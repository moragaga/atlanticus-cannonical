# ADA Command Center — Current Implementation

Estado: **VERIFIED / UPDATED 2026-09-21**

Corte auditado:

```text
moragaga/atlanticus@1c67212b21ef2241bcb59173ccb8e9cd237a0219
```

Parent inmediato:

```text
07eeb8d4ecc3f1e9d9a84ab1059eaad2fd5f78ce
```

## Físicamente en `main`

```text
scopes/ada-command-center/
├── backend/
│   ├── alarms/
│   │   ├── core/
│   │   └── persistence/
│   └── processes/
│       └── alarms-runtime/
└── web/
    └── alarms/
        └── configuration/
```

Clasificación:

```text
Backend Alarm Engine                              IMPLEMENTED / CURRENT
Alarm Configuration contract                     IMPLEMENTED / VERIFIED / CURRENT
Alarm Configuration Source/Release               IMPLEMENTED / VERIFIED / CURRENT
Alarm Configuration base Projection              IMPLEMENTED / VERIFIED / CURRENT
Alarm Configuration Manager integration          IMPLEMENTED / VERIFIED / CURRENT
Alarm Configuration document-mode Web surface    IMPLEMENTED / VERIFIED / CURRENT
Standalone Command Center application/shell      NOT YET IMPLEMENTED
Command Center Tool Catalog                      NOT YET IMPLEMENTED
B.2 ResolvedAlarmConfiguration                   NOT YET IMPLEMENTED
Runtime/Delivery materialization from B.2        NOT YET IMPLEMENTED
Management Projection                            NOT YET IMPLEMENTED
```

## Alarm Configuration CURRENT

Paquete:

```text
scopes/ada-command-center/web/alarms/configuration
```

La unidad editable/publicable es:

```text
AlarmConfiguration
├── rules: tuple[AlarmDefinition, ...]
└── messages: tuple[MessageDefinition, ...]
```

El contrato durable se expresa como documento serializable completo y se transforma a los
contratos ya existentes de `ada-command-center-alarms-core`.

No se agregó un segundo `AlarmDefinition` ni un DTO paralelo dentro de Alarm Core.

### Invariantes full-revision implementadas

- `AlarmIdentity` única en la revisión;
- `rule_name` único dentro de family;
- `rule_name` reutilizable entre families;
- `priority_order` único dentro de `priority_group`;
- IMPACT antes de RISK dentro del grupo;
- `message_key` único dentro del aggregate CURRENT;
- Message referenciado debe existir;
- Message FAMILY sólo puede ser consumido por la misma family;
- Message inactivo puede permanecer referenciado sin invalidar intrínsecamente la revisión;
- Special Condition referenciada debe existir;
- debe estar marcada `is_special_condition=true`;
- debe pertenecer a la misma family y `priority_group`.

La semántica CURRENT permanece:

```text
VALID
!=
READY
```

## Source / Release CURRENT

Alarm Configuration usa los contratos genéricos Atlanticus:

```text
SourceKey
SourceReleaseRef
SourceSnapshot
SourceStore
SourceService
History
exact release reads
```

La codec serializa el aggregate `Rules + Messages` como una única release.

La composición recibe `SourceStore` y `SourceKey` explícitamente. No fija todavía un provider
productivo Blob dentro de este paquete.

## Alarm Configuration Projection CURRENT

La Projection base materializa una `SourceRelease` exacta como `AlarmConfiguration`.

```text
Alarm Configuration SourceRelease exacta
→ AlarmConfigurationProjectionBuilder
→ ProjectionStore[AlarmConfiguration]
```

Invariante:

```text
ProjectionTarget.dependencies == ()
```

Esta Projection no es B.2 y no resuelve:

- Tool Catalog;
- evaluator availability;
- routing externo;
- visual targets externos;
- Runtime readiness;
- Delivery readiness.

## Manager integration CURRENT

Alarm Configuration reutiliza `atlanticus.web.manager` mediante un `ManagerModule` real.

La composición integra:

```text
workspace
validation
Source read/publication/history
base Projection
history preview
capability-local Web module
```

`SourceKey`, `access_key`, stores y principal son dependencias explícitas de la composición.

No se creó un Manager paralelo ni adapters legacy.

La Web capability-local usa actualmente un modo documental para editar/importar el aggregate
completo. El editor visual final de Rules/Messages/parameters permanece abierto.

## Evidencia de qualification del hito

Ejecutado en el checkout real por el usuario:

```text
uv sync                                OK
pytest                                 27 passed
ruff check .                           All checks passed
ruff format --check .                  corregido antes de publicación
```

El entorno de paquete ejecutó Python `3.14.2`.

## AlarmDefinition ya implementado

El código de Core conserva:

- AlarmDefinition;
- MessageDefinition;
- Message/Rule deactivation definitions;
- ReappearanceDefinition;
- AlarmEscalationDefinition;
- AlarmVisualTarget;
- evaluator + typed parameters;
- priority;
- business category;
- operational areas;
- semantic color;
- Special Condition;
- Message references.

No rediseñar estas capacidades desde cero.

## Base histórica ya disponible

### Journey

El motor genera eventos de:

- occurrence start/close;
- technical hold;
- management;
- reappearance;
- assignment/escalation;
- deactivation;
- priority suppression/release.

### Evidence

Conserva:

- occurrence;
- evaluated_at;
- status;
- evaluator/evidence contract;
- payload;
- errores técnicos;
- affected inputs.

Existe evidencia inicial, periódica, final y técnica/recovery.

Esto da una base real para análisis histórico profundo.
