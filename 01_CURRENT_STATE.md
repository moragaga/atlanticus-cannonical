# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

## Autoridad

```text
Implementation
moragaga/atlanticus@21cfb2f11362c1606ad14ff8adc7551948eced6a

Parent
6155dae407dc784114ff34c7b3b6f93125432713

Tree
48e4115a5fb53e64d83e2ae2243a9f11d612d26f

Canonical inspected before replacement
moragaga/atlanticus-cannonical@4058aab3525a09b568b80f3f6a5265e45e4f6fea

Historical decisions
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

Git permanece SOLO LECTURA.

## Estado resumido

```text
ADA-STORAGE-NAMESPACE                         CLOSED / VERIFIED / CURRENT
TOOL-PROJECTION-PERSISTENCE                   CLOSED / VERIFIED / CURRENT
TOOL-PERSISTENCE-RESILIENT-COMPOSITION        CLOSED / VERIFIED / CURRENT

ADA-WEB-KPI-COLLECTOR-CAPABILITY              CLOSED / VERIFIED / CURRENT

ADA-GENERIC-OPERATIONAL-BOOTSTRAP             PLANNED / NEXT
ADA-GENERIC-COLLECTOR-RUNTIME-WIRING          PLANNED / AFTER BOOTSTRAP

PYTHON-METADATA-ALIGNMENT                     PLANNED / SEPARATE
FULL-WORKSPACE-RUFF/CI                        UNVERIFIED / SEPARATE
```

## Hito cerrado

### Storage namespace

CURRENT:

```text
AdaStorageNamespace(
    application_namespace,
    tool_namespace,
)
```

Deriva:

```text
application_prefix
tool_prefix
local_application_root(base)
local_tool_root(base)
local_projection_root(base)
application_blob_name(relative)
tool_blob_name(relative)
```

Invariantes:

```text
container físico != namespace lógico
application namespace != tool namespace
SourceKey != deployment namespace
```

Topology aceptada:

```text
<data>/<application>/
├── users/
└── <tool>/
    ├── sources/
    └── projections/
```

`SourceStore` agrega `sources/`; la composición no lo agrega.

### Tool Projection durable

CURRENT:

```text
tool_projection_to_document
tool_projection_from_document

LocalToolProjectionStore
CosmosToolProjectionStore
```

Local:

```text
<base>/<application>/<tool>/projections
```

Cosmos:

```text
partition_key = <application>/<tool>
item identity = namespace + SourceKey
```

`ProjectionRecord.source_key` sigue siendo `SourceKey('tools')`.

### Tool persistence composition

CURRENT:

```text
ToolPersistenceSettings
ToolPersistenceComposition
compose_tool_persistence
resolve_active_tool_projection
project_current_tool_source
```

Providers:

```text
ToolSourceProvider.LOCAL | BLOB
ToolProjectionProvider.LOCAL | COSMOS
```

Estados de resolución:

```text
READY
UNCONFIGURED
UNAVAILABLE
INVALID
```

La composición es lazy respecto de I/O remoto: construirla no ejecuta health checks ni lecturas.

Runtime puede leer Projection activa aunque Source no esté disponible.

## Qualification observada

### Storage namespace

```text
uv lock                 PASS
pytest                   15 passed
ruff check               PASS
ruff format --check      PASS
git diff --check         PASS
```

### Tool Projection persistence

Observado antes del commit final:

```text
configuration codec tests   2 passed
projection-local tests      3 passed
projection-cosmos tests     5 passed
ruff check                  PASS
git diff --check            PASS
```

Los archivos publicados están en `6155dae407dc784114ff34c7b3b6f93125432713`.
No se observó en esta conversación un rerun completo posterior al último `ruff format`;
por tanto ese rerun final permanece `UNVERIFIED`.

### Tool persistence composition

Sobre el contenido publicado luego en `21cfb2f11362c1606ad14ff8adc7551948eced6a`:

```text
uv lock                 PASS
ruff format             PASS
ruff check              PASS
ruff format --check     PASS
pytest                  10 passed
git diff --check        PASS
```

## Frontera aún no cerrada

ADA Generic CURRENT todavía contiene:

```text
_StartupToolProjectionStore
resolve_current_tool_projection(...)
```

y ausencia de Tool Source current todavía produce:

```text
RuntimeError('Operational Tool source has no current release')
```

Eso no satisface todavía el contrato de startup resiliente de la aplicación.

Además, `__main__.py` continúa arrancando mediante:

```text
create_application_runtime()
run_web_application(runtime)
```

sin wiring de `ToolPersistenceComposition`.

Clasificación:

```text
ADA-GENERIC-OPERATIONAL-BOOTSTRAP
PLANNED / NEXT
```

## Regla congelada para el siguiente foco

```text
Web process must be able to exist
with all data,
with partial data,
or with no persisted Tool/KPI data.

Provider connection failure
must degrade the affected capability,
not automatically terminate the Web process.
```

No confundir:

```text
UNCONFIGURED
UNAVAILABLE
INVALID
```

con inexistencia del proceso Web.
