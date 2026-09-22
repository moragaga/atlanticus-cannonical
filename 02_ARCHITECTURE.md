# Atlanticus — Architecture

Estado: **CURRENT**

## Regla principal

Atlanticus es plataforma modular reusable.

ADA consume Atlanticus.

El núcleo genérico de Atlanticus no depende de ADA.

## ADA Command Center domain layer

CURRENT / IMPLEMENTED:

ADA Command Center distingue tres categorías en su scope:

```text
scopes/ada-command-center/
├── domain/
├── backend/
└── web/
```

`domain/` contiene contratos funcionales puros que tienen consumidores independientes en Web y Backend y no pertenecen exclusivamente a ninguna capa técnica.

Primer dominio implementado:

```text
scopes/ada-command-center/domain/alarms
```

Package:

```text
ada-command-center-alarms-domain
```

Namespace:

```python
ada_command_center.domain.alarms
```

Es autoridad CURRENT de:
- `AlarmIdentity`;
- `AlarmKind`;
- `Criticality`;
- Alarm Configuration authoring definitions;
- `AlarmConfiguration`;
- validación pura del aggregate;
- document encode/decode durable del aggregate;
- `AlarmConfigurationValidationError`.

Reglas CURRENT:

```text
Web -> Domain
Backend -> Domain
Domain -X-> Web
Domain -X-> Runtime/Persistence/Infrastructure
```

No usar `shared` como cajón genérico.

No duplicar DTOs equivalentes entre Web y Backend.

La migración fue un root replacement:
- `backend/alarms/core/definition.py` dejó de existir;
- `web/alarms/configuration/models.py` dejó de existir;
- no se conservaron aliases legacy para esas autoridades.

Engine runtime/lifecycle permanece en Backend.

Source/Projection/Manager permanece en Web.

Los artifacts Runtime-resolved y Delivery-resolved no pertenecen al authored domain.

Ver `14_ADA_COMMAND_CENTER/17_DOMAIN_OWNERSHIP_AND_MIGRATION.md`.

## Configuration / Administration

Manager administra Source/Projection sólo para dominios que realmente son Configuration Sources.

```text
Source      = Local | Blob
Projection  = Local | Cosmos
```

Los providers son ejes independientes.

```text
local + local
blob  + cosmos
blob  + local
local + cosmos
```

Projection representa un release exacto:

```text
ProjectionTarget
= SourceKey
+ SourceReleaseRef
+ dependencies
```

No reconstruir `ProjectionTarget` desde revision textual.

No mantener contratos paralelos para transición.

## Namespace de persistencia ADA

CURRENT:

```text
AdaStorageNamespace
├── application_namespace
└── tool_namespace
```

Separar siempre:

```text
connection
physical container
application namespace
tool namespace
SourceKey
```

Ejemplo lógico:

```text
conciencia_situacional/
├── users/
└── operaciones_integradas/
    ├── sources/
    └── projections/
```

`users` pertenece al nivel global de aplicación.

Los domains de Tool pertenecen a `<application>/<tool>`.

`SourceStore` agrega internamente `sources/<SourceKey>`.

## Tool Configuration / Projection

Tool Configuration es ADA-specific.

CURRENT:

```text
ToolSourceService
ToolProjectionBuilder
ProjectionRecord[ToolConfiguration]
LocalToolProjectionStore
CosmosToolProjectionStore
```

El namespace lógico de deployment no modifica `SourceKey`.

Cosmos Tool Projection usa:

```text
partition_key = <application>/<tool>
```

Local Tool Projection usa:

```text
<base>/<application>/<tool>/projections
```

## Tool persistence composition

Capability CURRENT:

```text
scopes/ada/web/tools/persistence
```

Composición:

```text
ToolPersistenceSettings
        ↓
compose_tool_persistence
        ↓
ToolPersistenceComposition
├── SourceStore
├── ProjectionStore[ToolConfiguration]
└── SourceProjectionService[ToolConfiguration]
```

Construir esta composición no debe abrir ni consultar servicios externos.

Operaciones separadas:

```text
resolve_active_tool_projection()
→ runtime read from durable Projection

project_current_tool_source()
→ Source current -> exact Projection
```

Estados:

```text
READY
UNCONFIGURED
UNAVAILABLE
INVALID
```

## Application availability boundary

Invariante congelada:

```text
APPLICATION EXISTENCE
!= CONFIGURATION EXISTENCE
!= INFRASTRUCTURE AVAILABILITY
!= DATA AVAILABILITY
```

Ausencia de Source/Projection/KPI data es estado funcional válido.

Falla de conectividad de una dependencia debe quedar confinada a esa capability.

Errores/contratos inválidos deben permanecer diagnosticables; resiliencia no significa ocultarlos.

## KPI Registry / Definition

Permanecen CURRENT sus contratos durables y exact dependencies.

## ADA KPI Collector CURRENT

Collector permanece una capability ADA Web separada.

```text
Latest Delivery Cosmos ─┐
                        ├─> AdaKpiCollector
Timeseries Delivery ────┘
ToolStructure ------------------┐
Tool projection revision -------┘
```

Fronteras congeladas:

```text
1 ToolComponent = 1 logical KPI Store
Subcomponent != Store
browser = cache only
Generic Application usable without Collector
```

## Command Center — próxima frontera

Después del cierre de Alarm Domain Extraction:

```text
B.2 — Materialization Contracts
PLANNED / NEXT
```

Target acordado:

```text
scopes/ada-command-center/backend/alarms/materialization
```

El primer incremento B.2 debe implementar contratos puros ya congelados, sin:
- I/O;
- stores;
- scheduler;
- process orchestration;
- Runtime Adoption;
- Live Delivery;
- Management Capture.
