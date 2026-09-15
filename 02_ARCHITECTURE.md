# Atlanticus — Architecture

Estado: **CURRENT**

## Regla principal

Atlanticus es plataforma modular reusable.

ADA consume Atlanticus.

El núcleo genérico de Atlanticus no depende de ADA.

## Planos principales

### Platform

Capacidades transversales:

- backend;
- connectivity;
- integrations;
- web.

`backend/` representa backend jobs y capacidades propias de esos jobs.

`web/` es frontera de primer nivel para Flask/Dash, JavaScript/CSS, composición Web, server-side Python con responsabilidad Web y capabilities Web reutilizables.

Connectivity es dual-use y no adquiere ownership funcional.

### Configuration / Administration

Manager administra configuración, authoring, validation, publication, history y projection actions.

Source genérico pertenece a:

```text
web/capabilities/source/
```

Projection genérica exact-release pertenece a:

```text
web/capabilities/projection/core
```

Manager consume estos contratos genéricos directamente. No mantiene una arquitectura paralela `legacy` vs `exact`.

### Operational Data

Operational Data conserva ownership separado para sources, producers, processes, planner y materialization.

### ADA Runtime

ADA Generic compone la experiencia operacional y consume capacidades Atlanticus.

ADA-specific authorization puede consumir/extender contratos genéricos, pero no convertirse en dependencia del core Atlanticus.

## Manager vs ADA Generic

```text
Manager      = administrar configuración
ADA Generic  = consumir configuración y materializar experiencia operacional
```

No comparten ownership de shell/header.

## Configuration vs Data

```text
CONFIGURATION DETERMINES EXISTENCE
DATA DETERMINES STATE
```

## Source vs Projection

Source y Projection son responsabilidades separadas.

```text
Source     = Local | Blob
Projection = Local | Cosmos | provider equivalente
```

Projection representa un `SourceReleaseRef` concreto.

Source current nunca se determina desde Cosmos.

## Contrato único de Manager

Cada `ManagerModule` declara:

```text
SourceKey
source_service
source_reader_service
projection_service
draft_validation_service
source_history_service | None
```

No existe una segunda familia `exact_*`.

### Source

```text
SourceReaderWorkflow
SourcePublicationWorkflow
SourceHistoryWorkflow
```

Todos transportan modelos de `source/core`:

```text
SourceSnapshot
SourceReleaseRef
HistoryPage
PublishResult
```

### Projection

Manager consume el servicio de Projection mediante:

```text
get_status(source_key)
select_current_target(source_key)
project(ProjectionTarget)
```

y conserva los modelos de `projection/core`:

```text
ProjectionStatus
ProjectionTarget
ProjectionExecutionResult
```

No existe adapter Manager hacia una identidad textual de revisión.

## Workspace genérico

`ManagerWorkspace` conserva:

```text
owner
payload local
SourceSnapshot como BASE
local revision
base payload revision
saved_at
```

Reglas:

- local revision identifica payload local;
- Source release identity permanece en `SourceSnapshot`;
- concurrency token no se convierte en release identity;
- schema vigente = `2`;
- no hay parser/shim legacy para workspace anterior.

## Concurrencia

Antes de publicar:

1. Manager verifica que la release current siga siendo la misma observada por el workspace.
2. Manager obtiene el snapshot current fresco.
3. si sólo cambió el concurrency token, utiliza el token fresco;
4. si cambió la release, falla como conflicto;
5. Source conserva la precondición autoritativa final.

No implementar merge automático sin contrato de dominio.

## Historical release -> WORKSPACE

Una release histórica no repunta current.

```text
historical payload
      ↓
local workspace on current BASE
      ↓
dirty local work
      ↓
validate → verify → publish
      ↓
new Source release
```

History conserva `SourceReleaseRef`.

## Web Capability Composition

Las capabilities mantienen ownership separado y dependencias explícitas.

Una composition se justifica cuando:

- capability A debe seguir siendo reusable sin B;
- capability B debe seguir siendo reusable sin A;
- el binding necesita conocer ambas;
- ninguna de las dos debe adquirir ownership de la otra.

No usar `web/compositions` como capa obligatoria ni como cajón general.

## Consumers de Manager

El core genérico ya está cerrado.

Cada dominio consumidor debe alinearse directamente al contrato único.

No crear arquitectura especial para:

- Navigation;
- Tools;
- KPI Configuration;
- KPI Definition.

Si un consumidor sigue usando nombres/servicios legacy, se reemplaza de raíz.

## Regla de reemplazo

Cuando una solución raíz reemplaza el contrato anterior:

```text
LEGACY                      REMOVE
ADAPTERS / SHIMS / ALIASES  FORBIDDEN
DOBLE CONTRATO              FORBIDDEN
revision -> ProjectionTarget reconstruction REMOVE
expected_source_revision    REMOVE
```

Compatibilidad durable histórica de un dominio sólo puede permanecer cuando sea un requisito explícito y esté separada del contrato activo.

## Fronteras futuras

OPEN / PLANNED:

- consumer cutover Navigation;
- consumer cutover Tools;
- consumer cutover KPI Configuration;
- consumer cutover KPI Definition;
- qualification global posterior a esos cutovers;
- demás frentes globales ya abiertos en sus documentos especializados.

No reabrir el contrato core de Manager para resolver consumidores.
