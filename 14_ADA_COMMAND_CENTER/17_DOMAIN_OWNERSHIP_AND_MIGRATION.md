# ADA Command Center — Domain Ownership and Alarm Configuration Migration

Estado: **CURRENT / IMPLEMENTED / QUALIFIED / CLOSED**

## 1. Authority checkpoint

Implementación CURRENT:

```text
moragaga/atlanticus:main
7a8c36a29860c8f010c3fe5b840c5f5af4d87d0f
```

Canonical base de este cierre:

```text
moragaga/atlanticus-cannonical:main
8f915fa75b6f4eaaa80fb003b290c613aa9ad735
```

Decisions consultado:

```text
moragaga/atlanticus-decisions:main
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

## 2. Resultado

El problema de ownership quedó resuelto mediante una capa transversal de dominio:

```text
scopes/ada-command-center/
├── domain/
│   └── alarms/
├── backend/
└── web/
```

Alarm Configuration:
- es editada/publicada desde Web;
- es consumida por backend;
- no pertenece exclusivamente a ninguna de esas capas;
- tiene una única autoridad contractual.

## 3. Package CURRENT

```text
scopes/ada-command-center/domain/alarms
```

Package:

```text
ada-command-center-alarms-domain==1.0.0
```

Namespace:

```python
ada_command_center.domain.alarms
```

Dependencias productivas:

```text
[]
```

El package no depende de:
- Dash;
- Manager;
- Web framework;
- Runtime process;
- Alarm persistence;
- Azure;
- Cosmos;
- Blob;
- SourceStore;
- ProjectionStore;
- job orchestration.

## 4. Authority CURRENT del dominio

Fundamentos:

```text
AlarmIdentity
AlarmKind
Criticality
```

Authoring definitions:

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

Aggregate:

```text
AlarmConfiguration
    rules: tuple[AlarmDefinition, ...]
    messages: tuple[MessageDefinition, ...]
```

Error de dominio:

```text
AlarmConfigurationValidationError
```

El aggregate conserva:
- identity uniqueness;
- `rule_name` uniqueness por Family;
- invariantes CURRENT de `priority_group`;
- Message key uniqueness;
- Rule -> Message scope/reference validation;
- Special Condition structural/reference validation CURRENT;
- `to_document()`;
- `from_document()`.

La migración no cambió semántica deliberadamente.

## 5. Estructura física CURRENT

```text
domain/alarms/
├── src/ada_command_center/domain/alarms/
│   ├── __init__.py
│   ├── models.py
│   ├── definition.py
│   ├── configuration.py
│   ├── errors.py
│   └── py.typed
├── commented/ada_command_center/domain/alarms/
│   └── mirror pedagógico equivalente
├── tests/
├── pyproject.toml
└── uv.lock
```

No se crearon subdominios artificiales debajo de `domain/alarms`.

Los módulos Python internos separan responsabilidades, pero forman un único package/domain.

## 6. Qué permanece fuera

Backend Alarm Engine conserva:
- `AlarmStatus`;
- `AlarmEvaluation`;
- `EvidenceSnapshot`;
- `PlannedAlarm`;
- routing/runtime state;
- occurrences/episodes;
- Management/Deactivation runtime;
- priority;
- lifecycle;
- journey;
- commit materialization.

Web Alarm Configuration conserva:
- Source/Release;
- Projection;
- Tool references;
- Manager workflows;
- workspace;
- Web authoring/presentation.

## 7. Dependency direction CURRENT

```text
                 ada_command_center.domain.alarms
                        /               \
                       v                 v
        Web Alarm Configuration    Backend Alarm Core
```

Además, Alarm Runtime declara Domain directamente donde consume sus contratos.

Configuration Manager declara Domain directamente donde construye/consume authored Alarm Configuration.

Reglas congeladas:

```text
Web Configuration -> Domain
Backend Alarm Core -> Domain
Runtime -> Domain cuando usa tipos de dominio
Domain -X-> Web
Domain -X-> Runtime
Domain -X-> Persistence
Domain -X-> Infrastructure
```

## 8. Root replacement CLOSED

Eliminadas como autoridades:

```text
scopes/ada-command-center/backend/alarms/core/
    src/ada_command_center/alarms/core/definition.py

scopes/ada-command-center/web/alarms/configuration/
    src/ada_command_center/web/alarms/configuration/models.py
```

Ambas están ausentes de `main@7a8c36a...`.

No existen aliases/re-exports de compatibilidad destinados a preservar esas autoridades legacy.

Los consumers fueron actualizados en el mismo incremento.

## 9. Tests y ownership

Los tests de:
- shared fundamentals;
- Alarm definitions;
- `AlarmConfiguration`;
- cross-Rule validation;
- document round-trip;

viven bajo:

```text
scopes/ada-command-center/domain/alarms/tests
```

Engine conserva tests de runtime/lifecycle.

Web conserva tests de Source/Projection/Manager/Tool references/UI authoring.

## 10. Qualification del hito

Qualification observada durante integración:

```text
Alarm Domain
49 passed

Backend Alarm Core
179 passed

Alarms Runtime
16 passed

Web Alarm Configuration
28 passed

Configuration Manager
11 passed
```

Total:

```text
283 passed
```

Para cada package afectado también quedaron GREEN:

```text
ruff check .
ruff format --check .
```

Además:

```text
Alarm Domain extraction static verification passed
git diff --check
```

Los mirrors pedagógicos fueron sincronizados y sus tests de equivalencia quedaron GREEN.

No se capturó una rerun completa posterior al commit final; si se requiere un gate formal sobre bytes exactos del SHA final, queda UNVERIFIED.

## 11. B.2 ownership después de la extracción

Target acordado para pure materialization:

```text
scopes/ada-command-center/backend/alarms/materialization
```

Responsabilidad futura:

```text
AlarmConfiguration
+ qualified external inputs
        |
        v
AlarmConfigurationResolution
```

Target acordado para process orchestration:

```text
scopes/ada-command-center/backend/processes/alarms-materialization
```

Ambos paths están ausentes todavía de `main@7a8c36a...`.

## 12. Separación congelada

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

No mover artifacts operacionales al authored Domain.

## 13. SUPERSEDED

Quedan reemplazadas las propuestas previas de ubicar el contrato compartido bajo:

```text
scopes/ada-command-center/backend/alarms/configuration
```

o:

```text
scopes/ada-command-center/alarms/configuration/core
```

Authority CURRENT:

```text
scopes/ada-command-center/domain/alarms
```

También queda reemplazada la organización histórica donde:
- authoring definitions pertenecían físicamente a Alarm Core;
- `AlarmConfiguration` pertenecía físicamente a Web.

## 14. Conflicto fuera de alcance

Project baseline:

```text
Python 3.14.7
```

Package CURRENT:

```text
requires-python ==3.14.2
```

La migración no corrigió este conflicto.

## 15. Foco único siguiente

```text
B.2 — Materialization Contracts
```

Primer incremento:

```text
scopes/ada-command-center/backend/alarms/materialization
```

Implementar sólo contratos puros ya acordados.

No implementar todavía:
- resolver B.2 completo;
- I/O;
- stores;
- scheduler;
- process orchestration;
- Runtime Adoption;
- Live Delivery;
- Management Capture.
