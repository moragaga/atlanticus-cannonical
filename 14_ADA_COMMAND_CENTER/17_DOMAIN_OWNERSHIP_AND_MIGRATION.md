# ADA Command Center — Domain Ownership and Alarm Configuration Migration

Estado: **PROJECT CONTRACT AGREED / MIGRATION NOT YET IMPLEMENTED**

## 1. Authority checkpoint

Implementación auditada durante este cierre:

```text
moragaga/atlanticus:main
ebf736a1cf5193a297fbafc55c5c11ca9993f24c
```

Canonical base de este delta:

```text
moragaga/atlanticus-cannonical:main
b85e6b2b27e39d0531b95f3fbe97ef6c8fd06949
```

Decisions consultado:

```text
moragaga/atlanticus-decisions:main
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

Este documento distingue explícitamente:

```text
VERIFIED / CURRENT
PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED
PROPOSED IMPLEMENTATION ORDER
OPEN
SUPERSEDED
```

## 2. Problema de ownership

VERIFIED / CURRENT:

```text
scopes/ada-command-center/
├── backend/
│   ├── alarms/
│   │   ├── core/
│   │   └── persistence/
│   ├── processes/
│   │   └── alarms-runtime/
│   └── tools/
│       └── catalog/
└── web/
    └── alarms/
        └── configuration/
```

El modelo editable de Alarm Configuration está físicamente repartido:

```text
backend/alarms/core/definition.py
    -> AlarmDefinition
    -> MessageDefinition
    -> authoring enums/definitions

web/alarms/configuration/models.py
    -> AlarmConfiguration aggregate
    -> cross-Rule validation
    -> document serialization/deserialization
```

Esto no refleja correctamente el ownership funcional.

Alarm Configuration:
- es editada y publicada desde Web;
- es validada/materializada por backend B.2;
- define contratos funcionales compartidos por ambos lados;
- no es una responsabilidad exclusiva de Web;
- no es una responsabilidad exclusiva del Runtime backend.

Por tanto:

```text
Alarm Configuration domain contract
!= Web contract
!= Runtime contract
```

## 3. Decisión de estructura transversal

PROJECT CONTRACT AGREED:

Los contratos funcionales compartidos de ADA Command Center viven bajo una capa de dominio explícita:

```text
scopes/ada-command-center/
├── domain/
│   └── alarms/
│
├── backend/
│   ├── alarms/
│   ├── processes/
│   └── tools/
│
└── web/
    ├── alarms/
    └── application/
```

La raíz `domain/` evita mezclar en el primer nivel:
- capas técnicas (`web`, `backend`);
- dominios funcionales (`alarms`, futuros dominios transversales).

No usar `shared` como cajón genérico.

No usar `contracts` como único concepto porque el dominio contiene también invariantes y validación pura.

No usar `core` como categoría raíz genérica.

## 4. Package target

PROJECT CONTRACT AGREED:

```text
scopes/ada-command-center/domain/alarms
```

Package conceptual:

```text
ada-command-center-alarms-domain
```

Namespace Python:

```python
ada_command_center.domain.alarms
```

El package debe ser puro:

```text
MUST NOT depend on:
- Dash
- Manager
- Web framework
- Runtime process
- Alarm persistence
- Azure
- Cosmos
- Blob
- SourceStore
- ProjectionStore
- job orchestration
```

## 5. Contenido target del dominio

### 5.1 Fundamentos transversales

El dominio debe ser autoridad de los tipos que usan tanto authoring como Engine:

```text
AlarmIdentity
AlarmKind
Criticality
```

No duplicarlos entre Web y Backend.

### 5.2 Authoring definitions

Mover al dominio:

```text
BusinessCategory
AlarmColor
OperationalArea
VisibilityMode
MessageScope
ProcessAlarmProjectionMode

AlarmDeactivationDefinition
MessageDeactivationDefinition
ReappearanceDefinition

AlarmEscalationStepDefinition
AlarmEscalationDefinition

AlarmVisualSubcomponentTarget
AlarmVisualTarget

MessageDefinition
AlarmDefinition
```

### 5.3 AlarmConfiguration aggregate

Mover al dominio:

```text
AlarmConfiguration
    rules: tuple[AlarmDefinition, ...]
    messages: tuple[MessageDefinition, ...]
```

El aggregate conserva ownership de:
- unicidad de `AlarmIdentity`;
- `rule_name` único por Family;
- invariantes de `priority_group` que sigan vigentes;
- Message key uniqueness;
- Rule -> Message scope/reference validation;
- Special Condition structural/reference validation aplicable al authoring;
- `to_document()`;
- `from_document()`.

La serialización documental es parte del contrato durable de Alarm Configuration, no de la UI.

### 5.4 Errores de dominio

Mover:

```text
AlarmConfigurationValidationError
```

No mover:

```text
AlarmConfigurationSourceError
```

porque pertenece al boundary Source/Web infrastructure.

## 6. Qué NO pertenece al dominio transversal

Permanecen en Backend Alarm Engine:

```text
AlarmStatus
AlarmEvaluation
EvidenceSnapshot
PlannedAlarm
AlarmRouting
AlarmOccurrence
AlarmEpisode
AlarmRuntimeState
RuntimeEvaluationState

ManagementAction
ManagementEffect
DeactivationIntent
DeactivationRequest
DeactivationEffect

priority resolution
routing runtime
lifecycle
journey
evidence persistence materialization
Engine commits
```

Permanecen en Web Alarm Configuration:

```text
source_release.py
source_projection.py
tool_references.py
manager.py
workflows.py
workspace.py
web/*
```

La Web consume el dominio; no redefine DTOs equivalentes.

## 7. Dependency direction

PROJECT CONTRACT AGREED:

```text
                 ada_command_center.domain.alarms
                         pure domain
                        /           \
                       /             \
                      v               v
        Web Alarm Configuration    Backend Alarm Core
                      \               /
                       \             /
                        v           v
                    B.2 Materialization
```

Reglas:

```text
Web Configuration -> Domain
Backend Alarm Core -> Domain
B.2 Materialization -> Domain + Engine contracts + qualified external inputs
```

Prohibido:

```text
Domain -> Web
Domain -> Runtime process
Domain -> Persistence
B.2 -> ada_command_center.web.*
Web Configuration -> Backend Engine sólo para obtener authoring DTOs
```

## 8. Root replacement

PROJECT CONTRACT AGREED:

La migración es un reemplazo de raíz, no una etapa con aliases legacy permanentes.

Targets CURRENT que dejan de ser autoridad:

```text
scopes/ada-command-center/backend/alarms/core/
    src/ada_command_center/alarms/core/definition.py

scopes/ada-command-center/web/alarms/configuration/
    src/ada_command_center/web/alarms/configuration/models.py
```

Target authority:

```text
scopes/ada-command-center/domain/alarms/
    src/ada_command_center/domain/alarms/...
```

No mantener re-exports de compatibilidad del tipo:

```text
ada_command_center.alarms.core.AlarmDefinition
ada_command_center.web.alarms.configuration.AlarmConfiguration
```

si su única finalidad es preservar imports legacy.

Todos los consumers se actualizan en el mismo incremento integrable.

## 9. Tests y ownership

Los tests deben seguir al contrato que prueban.

Mover conceptualmente al nuevo package de dominio los tests que hoy prueban:
- `AlarmDefinition` y authoring definitions;
- `AlarmConfiguration` aggregate;
- cross-Rule validation;
- document round-trip.

Ejemplos CURRENT que cambian de owner:

```text
backend/alarms/core/tests/test_definition.py
web/alarms/configuration/tests/test_models.py
```

Permanecen en Web los tests de:
- Source publication/read;
- Projection;
- Manager workflows;
- workspace;
- Tool reference assistance;
- UI/structured authoring behavior.

Permanecen en Engine los tests de:
- lifecycle;
- priority;
- routing;
- Management;
- deactivation runtime;
- persistence/recovery.

No crear tests cuya única finalidad sea congelar paths internos más allá de las fronteras arquitectónicas contractuales.

## 10. B.2 ownership después de la extracción

PROJECT CONTRACT AGREED:

Separar resolución determinista de orquestación operacional.

### 10.1 Pure materialization capability

Target:

```text
scopes/ada-command-center/backend/alarms/materialization
```

Responsabilidad:

```text
AlarmConfiguration
+ Confirmed Tool Catalog snapshot
+ Tool reconciliation qualification
+ evaluator qualification
        |
        v
AlarmConfigurationResolution
```

Aquí viven los contratos/resolución B.2:

```text
AlarmResolutionKey
AlarmResolutionFinding
AlarmConfigurationResolution
RuntimeAlarmConfiguration
DeliveryAlarmConfiguration
ResolvedDeliveryAlarm
ResolvedDeliveryMessage
ResolvedDeactivationPolicy
ResolvedAlarmVisualTarget
...
```

No hace I/O físico y no conoce scheduler/job runtime.

### 10.2 Materialization process

Target:

```text
scopes/ada-command-center/backend/processes/alarms-materialization
```

Responsabilidad:
- adquirir inputs por puertos explícitos;
- comparar revisiones;
- ejecutar el resolver B.2;
- persistir artifacts/findings mediante stores explícitos;
- emitir diagnostics del proceso.

No convierte Alarm Runtime en:
- downloader de SharePoint;
- Tool discovery;
- Message catalog resolver;
- cross-configuration validator.

## 11. Alarm Configuration acquisition para B.2

VERIFIED:

`ProjectionStore[AlarmConfiguration]` conserva provenance de Source mediante `ProjectionRecord`:

```text
source_release_id
source_published_at_utc
payload
```

Por tanto B.2 puede consumir la Projection activa sin volver a SharePoint para construir la identidad del candidato.

Conceptualmente:

```text
Alarm Configuration active Projection
+ ToolCatalogSnapshot
        |
        v
AlarmResolutionKey(
    alarm_configuration_revision = projection.source_release_id,
    confirmed_tool_catalog_revision = tool_catalog.revision,
)
```

La Projection sigue siendo un release exacto; no reconstruir provenance desde un texto sin autoridad.

## 12. Web ownership después de la migración

La Web continúa siendo dueña de la experiencia de configuración:

```text
browser/editor
Manager workflow
Source publish/history
Projection orchestration para authoring/read model
Tool reference assistance
```

Pero trabaja sobre el mismo aggregate de dominio que B.2 consume.

Por tanto:

```text
UI edits AlarmConfiguration
Source publishes AlarmConfiguration
Projection materializes exact AlarmConfiguration release
B.2 consumes AlarmConfiguration
```

No existe un DTO Web distinto que luego deba transformarse a un DTO backend equivalente.

## 13. Runtime-resolved y Delivery-resolved son contratos distintos

No colocar bajo `domain/alarms` los artifacts operacionales simplemente porque también contienen “configuration”.

Separar:

```text
AUTHORED DOMAIN
AlarmConfiguration
AlarmDefinition
MessageDefinition

RUNTIME RESOLVED
RuntimeAlarmConfiguration
PlannedAlarm

DELIVERY RESOLVED
DeliveryAlarmConfiguration
ResolvedDeliveryAlarm
```

El dominio transversal describe lo que se configura.

B.2 materialization describe cómo una revisión válida se convierte en artifacts operacionales.

## 14. Orden de migración recomendado

PROPOSED IMPLEMENTATION ORDER / NEXT:

### Incremento 1 — Domain extraction

```text
create domain/alarms package
move shared fundamentals + authoring definitions
move AlarmConfiguration aggregate/validation/document contract
update Web imports
update Engine imports
move relevant tests
remove old authorities
run package/workspace gates
```

Objetivo: cero cambio semántico funcional.

### Incremento 2 — B.2 materialization contracts

Crear:

```text
backend/alarms/materialization
```

con DTOs/contratos ya acordados y sin I/O.

### Incremento 3 — Pure B.2 resolver

Implementar resolution/validation contra inputs explícitos.

### Incremento 4 — alarms-materialization process

Crear orquestación de adquisición/persistencia.

### Incrementos posteriores

```text
Runtime Adoption / Effective Head implementation
Live Delivery implementation
Management Capture implementation
```

No mezclar todos estos cambios en el primer incremento.

## 15. Invariantes congelados para la migración

```text
1. Alarm Configuration es dominio transversal de ADA Command Center.
2. No pertenece exclusivamente a Web ni Backend.
3. Web y Backend consumen una sola representación contractual.
4. No duplicar DTOs Web/Backend.
5. Domain package es puro y no conoce infraestructura.
6. Engine runtime/lifecycle permanece fuera del domain package.
7. Source/Projection/Manager permanece fuera del domain package.
8. Root replacement: no aliases legacy ni adapters temporales permanentes.
9. B.2 pure resolution != B.2 process orchestration.
10. Runtime no adquiere SharePoint/Tool catalogs por conveniencia.
11. Projection release provenance se conserva explícitamente.
12. Authored, Runtime-resolved y Delivery-resolved son contratos distintos.
13. Primer incremento de implementación debe ser sólo Domain extraction.
```

## 16. SUPERSEDED

Quedan reemplazadas las propuestas previas de ubicar el contrato compartido directamente bajo:

```text
scopes/ada-command-center/backend/alarms/configuration
```

o:

```text
scopes/ada-command-center/alarms/configuration/core
```

Target acordado:

```text
scopes/ada-command-center/domain/alarms
```

También queda descartada la idea de que B.2 importe `ada_command_center.web.*` para obtener Alarm Configuration.

## 17. OPEN después de esta decisión

No bloquean el diseño de ownership:
- estructura física fina de módulos dentro de `domain/alarms` (`models.py`, `definition.py`, `configuration.py`, etc.); puede ajustarse durante implementación sin cambiar ownership;
- contrato/fuente concreta de current Tool reconciliation GREEN;
- stores físicos de READY/BLOCKED/artifacts;
- owner/package final de Live Delivery;
- cause-template/evaluator evidence schema;
- `shift_end` provider;
- Runtime Adoption persistence details;
- cadence/retention/deployment topology.

Conflict visible fuera de alcance:

```text
Project baseline = Python 3.14.7
CURRENT Command Center packages audited = requires-python ==3.14.2
```

No corregir ese baseline dentro del Domain extraction salvo que bloquee el incremento; tratarlo como cambio separado.

## 18. Foco único siguiente

```text
Command Center — Alarm Domain Extraction
```

Objetivo del siguiente incremento:

```text
mover ownership sin cambiar semántica
```

No implementar B.2 funcional en el mismo incremento.
