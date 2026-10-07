# Atlanticus — Architecture

Estado: **CURRENT — MODULAR RUNTIME + CONSUMER-OWNED DISTRIBUTED DEPLOYMENT RESOURCES**

## Regla principal

Atlanticus es modular y reusable. ADA y Command Center son consumidores.

## Clean cutover rule

```text
contracts before consumers
clean replacement
no legacy aliases
no dual source of truth
no compatibility storage/deployment path without explicit decision
```

## Operational Data

El contrato CURRENT previo permanece:

```text
DataInputSpec
    ↓
DataInputPlanner
    ↓
DataInputLoadPlan
    ↓
DataInputLoader
    ↓
LoadedDataInputs
    ↓
DataInputContext
```

## Distributed process resource boundary — FROZEN

Resource sizing pertenece al deployment consumer, no al proceso Python.

```text
process artifact
    identity / dependencies / entrypoint
          │
          │ no CPU/RAM authority
          ↓
distribution generation
          ↓
deployment.resources.json
          ↓
effective consumer-owned sizing
          ├── up / run → ephemeral Compose override
          └── simulate → scheduler docker run
```

### Source of truth

Única fuente persistente efectiva:

```text
deployment.resources.json
```

No son autoridad:

```text
pyproject.toml [tool.atlanticus.container.resources]
base compose.yaml
base compose.bind.yaml
temporary override files
simulation.json
```

Las proyecciones derivadas pueden materializar recursos para una ejecución, pero no se convierten en configuración persistente.

## Resource contract — FROZEN

```text
schema_version = 1

processes.<alias>.vcpu
processes.<alias>.memory_gib
```

Invariantes:

```text
0.25 <= vcpu <= 4.0
vcpu step = 0.25
memory_gib = vcpu × 2
all installed process aliases appear exactly once
no unknown aliases
```

Default para proceso nuevo:

```text
0.5 vCPU / 1.0 GiB
```

Docker memory projection:

```text
MiB = memory_gib × 1024
```

## Consumer ownership — FROZEN

El desarrollador/consumer puede cambiar `deployment.resources.json`.

Regeneración:

```text
retained process
→ preserve existing resource pair

new process
→ initialize default pair

removed process
→ remove resource entry with regenerated composition
```

No editar Compose para persistir sizing.

## Extension integration boundary — IMPLEMENTED / QUALIFICATION OPEN

La implementación actual de `integrate`:

```text
valid current distribution
+ extension processes
→ merge manifest/services
→ preserve resource entries
→ add default resource entry per new alias
→ regenerate base compose structure
→ validate candidate
→ publish managed files atomically
```

La qualification específica de preservación/rollback de resources durante integración permanece abierta.

## Docker runtime-input boundary — CURRENT

El Dockerfile usa:

```text
COPY processes/${FILENAME}/ ./
```

pero `.dockerignore` actúa como allowlist.

Permitido:

```text
pyproject.toml
uv.lock
wheels/
src/
secrets.json
config/connections.json
```

Excluido:

```text
.env
config.json
*.detail
```

El local workspace debe respetar la misma frontera.

## Generic Web capabilities

El resto de capabilities genéricas ya documentadas permanece sin cambio por este hito.
