# Artifact and Distribution Boundary

Estado: **CURRENT — DISTRIBUTED RESOURCE CONTRACT IMPLEMENTED / FULL ARTIFACT QUALIFICATION STILL PLANNED**

## Process distribution CURRENT

A process distribution separates:

```text
transport artifacts
consumer configuration
deployment resource sizing
local execution projections
```

### Transport

Process transport artifact contains package/runtime inputs and detail templates.

### Consumer configuration

Active consumer files remain installation-owned:

```text
.env
config.json
secrets.json
config/connections.json
```

### Deployment resources

Effective process sizing is installation-owned:

```text
deployment.resources.json
```

It is not read from `pyproject.toml`.

## Resource contract

```json
{
  "schema_version": 1,
  "processes": {
    "<alias>": {
      "vcpu": 0.5,
      "memory_gib": 1.0
    }
  }
}
```

Invariants:

```text
vCPU 0.25..4.0
step 0.25
RAM GiB = 2 × vCPU
exact alias set
```

## Docker projection

Base Compose does not persist CPU/RAM.

```text
up/run
→ read deployment.resources.json
→ validate
→ render temporary Compose override
→ execute
→ delete override
```

Simulation resolves from the same file.

## Regeneration

```text
existing alias
→ preserve resource pair

new alias
→ 0.5 / 1.0 default
```

A resource declaration in process `pyproject.toml` does not override this behavior.

## Extension integration CURRENT implementation

`integrate` merges a new component into a CURRENT distribution and also updates the resource file.

Implementation flow:

```text
read current resource map
add new alias with default_resources()
write candidate resource file
validate candidate distribution
publish deployment.resources.json with managed files
```

Focused integration qualification remains OPEN.

## Resource guide

Generated distribution root contains:

```text
AZURE_CONTAINER_APPS_RESOURCES.md
```

It documents allowed vCPU/RAM pairs and Docker MiB translation.

## Full artifact qualification

Still PLANNED:

```text
generate every expected current-head artifact
verify package/file set
verify metadata/dependencies
verify installability/consumer use
detect stale contents
isolated consumer qualification
```
