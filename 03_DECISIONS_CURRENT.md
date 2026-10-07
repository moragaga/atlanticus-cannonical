# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global — FROZEN

```text
uv; no pip normal
contracts before consumers
backend before frontend
clean root cutover
no legacy adapters/shims/aliases
one focus per increment
Git read-only unless explicit authorization
```

## Distributed resource ownership — FROZEN / CLOSED

Decisión CURRENT:

```text
deployment.resources.json
= unique persistent effective resource source for a distributed process set
```

El archivo pertenece al consumidor.

La configuración funcional del proceso no debe absorber CPU/RAM de deployment.

## Resource pairs — FROZEN

Atlanticus acepta para esta frontera únicamente:

```text
vCPU: 0.25 .. 4.0
step: 0.25
RAM GiB = vCPU × 2
```

Default:

```text
0.5 vCPU / 1.0 GiB
```

Docker:

```text
memory_mib = memory_gib × 1024
```

## Superseded resource decisions

Quedan SUPERSEDED:

```text
pyproject.toml resources
→ effective distributed sizing

manual edit of generated compose
→ persistent sizing

update-deployment command
→ synchronize JSON into Compose

base Compose cpus/mem_limit
→ resource authority
```

CURRENT:

```text
edit deployment.resources.json
→ next up/run/simulate reads it automatically
```

## Compose projection — FROZEN

Base Compose mantiene estructura de servicios.

`up` y `run` generan un override temporal de recursos.

El override:

```text
is derived
is ephemeral
is not consumer configuration
is deleted after command execution
```

## Simulation — FROZEN

Simulation no vuelve a leer recursos desde `pyproject.toml`.

Recibe exactamente los valores resueltos desde `deployment.resources.json`.

## Regeneration — FROZEN

```text
existing consumer sizing
→ preserve

new process without existing sizing
→ default_resources()

pyproject resource metadata
→ ignored for distributed sizing
```

## Extension integration — CURRENT IMPLEMENTATION / QUALIFICATION OPEN

`integrate` ya incorpora `deployment.resources.json` al candidato y agrega defaults a aliases nuevos.

Aún debe cerrarse una qualification enfocada que demuestre explícitamente:

```text
custom existing sizing preserved
new alias gets default
invalid existing contract blocks integration without mutation
resource publication failure rolls back
```

No crear compatibility path para distribuciones legacy sin decisión explícita.

## Docker runtime-input contract — REFINED / CURRENT

La regla anterior que trataba todo `secrets.json` como input prohibido queda SUPERSEDED.

CURRENT allowlist:

```text
secrets.json
config/connections.json
```

siguen siendo runtime inputs admitidos en imagen.

Continúan excluidos:

```text
.env
config.json
*.detail
```

## Operational Data / KPI / Alarm

Las decisiones previas vigentes de esos frentes no fueron reabiertas por este hito.

## Next

Único foco recomendado:

```text
EXTENSION-RESOURCE-INTEGRATION-QUALIFICATION
```
