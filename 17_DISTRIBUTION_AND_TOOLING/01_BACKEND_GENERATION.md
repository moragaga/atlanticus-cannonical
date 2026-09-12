# Backend Generation

Estado: **CURRENT DIRECTION**

## Objetivo

Existe una diferencia entre:

1. crear/validar el contrato de un proceso;
2. materializar el proceso productivo;
3. generar un artifact listo para que otra plataforma lo distribuya.

No mezclar esas responsabilidades.

## Flujo

```text
Process Contract
      ↓
Test/Qualification Process
      ↓
GREEN
      ↓
Final Process Materialization
      ↓
Artifact
      ↓
Distribution Input
```

## Test/Qualification Process

Permite probar aisladamente:

- contract;
- inputs/outputs;
- runtime policy;
- external services;
- configuration;
- failure modes;
- observability;
- health/readiness.

No debe ser una implementación paralela que luego se abandona.

La misma definición aprobada alimenta el proceso final.

## Final Process

Debe materializar todo lo necesario para ejecución/distribución, por ejemplo según el proceso:

- package/entrypoint;
- dependency lock;
- runtime metadata;
- env/detail contract;
- secrets references;
- Docker build contract;
- support service requirements;
- health/readiness;
- run-once/continuous semantics;
- commented mirror donde aplique;
- tests/gates requeridos.

## DevOps boundary

Atlanticus **NO es owner del pipeline corporativo**.

No congelar:

- stages corporativos;
- naming de pipelines;
- service connections;
- approvals;
- release strategy externa.

Atlanticus sí debe producir un **pipeline-ready distribution artifact** cuyo contrato sea claro para DevOps.

```text
Atlanticus owns:
Artifact + distribution contract

DevOps owns:
Pipeline implementation/execution
```
