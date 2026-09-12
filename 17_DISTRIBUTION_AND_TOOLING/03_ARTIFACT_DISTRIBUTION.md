# Artifact and Distribution Boundary

Estado: **CURRENT**

## Pipeline interno Atlanticus

```text
SOURCE
→ ARTIFACT
→ DISTRIBUTION INPUT
```

No confundir `distribution` con pipeline corporativo.

## Estado actual verificado

`deployment/processes/bundle.py` es el bundler central de procesos.

`scripts/local-process.sh` ya permite:

- prepare por `--all`;
- prepare por process;
- prepare por scope;
- validate;
- build;
- up/down;
- logs;
- run `<process>` con `--run-once`.

Eso se conserva como base y no se reemplaza por otro framework.

## Dirección

Cada generador debe terminar en artifacts que puedan ser consumidos por:

- local Docker;
- distribution packaging;
- pipeline externo.

## Regla

La misma definición no debe tener una implementación distinta para:

- local;
- dev;
- pipeline.

Cambian adapters/configuración/distribution environment, no el contrato funcional.
