# Atlanticus — Current State

Estado: **CURRENT — DYNAMIC PROCESS DEPLOYMENT RESOURCE BOUNDARY CLOSED / VERIFIED LOCALLY**

## Autoridad

```text
Implementation
moragaga/atlanticus@5c40faed4df7f3d7b6db79251144a9ec09e09e91

Canonical base before replacement
moragaga/atlanticus-cannonical@15a51f70396726a2ad3b88d1afc66ce8cfff3300
```

## CLOSED / VERIFIED en este hito

La distribución de procesos tiene una fuente efectiva de recursos separada del artifact y del proceso Python:

```text
deployment.resources.json
    ↓
validation
    ↓
Docker execution projection
```

Contrato por proceso:

```json
{
  "vcpu": 0.5,
  "memory_gib": 1.0
}
```

Default global:

```text
0.5 vCPU / 1.0 GiB
```

Parejas válidas:

```text
vCPU 0.25 .. 4.0
step 0.25
memory_gib = vCPU × 2
```

Traducción Docker:

```text
memory MiB = memory_gib × 1024
```

Ejemplo:

```text
1.5 vCPU / 3.0 GiB
→ cpus: 1.5
→ mem_limit: 3072m
```

## Ownership CURRENT

```text
pyproject.toml
    process/package/container identity
    NOT resource authority for distributed sizing

deployment.resources.json
    consumer-owned effective deployment sizing

deployment/local/compose*.yaml
    generated structural deployment
    NOT persistent sizing authority
```

La distribución inicial crea `deployment.resources.json`.

Una regeneración conserva valores existentes de los procesos retenidos y usa el default sólo para procesos sin sizing previo.

`AZURE_CONTAINER_APPS_RESOURCES.md` se entrega en la raíz de la distribución como guía del contrato admitido.

## Ejecución local CURRENT

`up` y `run` leen los recursos en cada ejecución y generan un Compose override temporal.

```text
base compose
+ ephemeral resources override
→ docker compose
```

El override no se conserva como segunda fuente de verdad.

`simulate` recibe CPU/RAM desde el mismo `deployment.resources.json` y los proyecta al scheduler, que termina ejecutando Docker con `--cpus` / `--memory`.

## Local Docker runtime inputs CURRENT

Se corrigió una inconsistencia preexistente entre Dockerfile, `.dockerignore`, gate y local workspace.

CURRENT:

```text
image/runtime inputs allowed
    pyproject.toml
    uv.lock
    wheels/
    src/
    secrets.json
    config/connections.json

excluded
    .env
    config.json
    *.detail
```

`deployment/local/generate_compose.py` conserva ahora `secrets.json` y `config/connections.json` en el workspace local.

## Extension integration

IMPLEMENTED en `consumer/process.py`:

```text
existing deployment.resources.json
    ↓ read
existing entries preserved
    +
new process → default_resources()
    ↓
write candidate deployment.resources.json
```

`deployment.resources.json` forma parte del conjunto administrado/publicado por `integrate`.

Sin embargo, la qualification específica de esta nueva frontera de integración todavía no está cerrada.

## Qualification observada

Gate `tooling/gates/process-deployment/check.py` reportado GREEN:

```text
Ruff check / format                   PASS
deployment/processes/tests            31 passed
deployment/local/tests                16 passed
tooling/tests/local/processes          8 passed
tooling/tests/distribution/processes  56 passed
shell launcher syntax                 PASS

total tests                           111 passed
```

La qualification es local reportada por el usuario.

## UNVERIFIED / DEFERRED

```text
real Docker smoke proving an edited resource value is enforced by Docker
Azure runtime resource qualification
```

Estos puntos no bloquean el cierre de este incremento por decisión del usuario.

## OPEN

```text
focused extension/resource integration qualification
full current-head artifact generation/qualification
.env.detail exhaustive audit
isolated distributed consumer qualification
Python 3.14.7 / Trixie migration
production Azure / Entra qualification
```

## NEXT único

```text
ATLANTICUS-EXTENSION-RESOURCE-INTEGRATION-QUALIFICATION
```
