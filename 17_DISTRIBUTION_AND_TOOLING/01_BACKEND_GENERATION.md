# Backend Generation

Estado: **CURRENT GENERIC PROCESS DISTRIBUTION MECHANICS / SCOPE-SPECIFIC NORMALIZATION STILL PLANNED**

## Boundary

Separate:

```text
backend/process implementation
scope-specific distribution composition
generic process distribution mechanisms
external DevOps pipeline
```

## Generic process distribution CURRENT

Root tooling currently owns reusable process distribution mechanics:

```text
tooling/distribution/processes
deployment/processes
deployment/local
```

This includes:

```text
process bundle composition
distribution manifests/services
consumer tooling
extension packages
local Compose
simulation
deployment.resources.json
resource guide
validation gate
```

## Resource ownership CURRENT

Process metadata owns:

```text
command
system-profile
package/runtime identity
```

The distributed consumer owns effective CPU/RAM through:

```text
deployment.resources.json
```

Do not reintroduce resource authority into per-process `pyproject.toml`.

## Scope-specific direction

For a distributable scope:

```text
scopes/<scope>/backend
    backend packages/processes

scopes/<scope>/tooling/distribution/backend
    scope-specific composition/qualification when real scope-specific behavior requires it
```

Do not move generic process distribution mechanics into ADA-specific tooling.

## PLANNED

Scope-specific backend tooling normalization remains separate from the resource boundary closed here.

## DevOps boundary

Atlanticus owns:

```text
artifact + distribution contract
```

External DevOps owns:

```text
pipeline implementation/execution
```
