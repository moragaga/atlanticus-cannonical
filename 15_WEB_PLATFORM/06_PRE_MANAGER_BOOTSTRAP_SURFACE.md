# Web Platform — Login / Bootstrap Console

Estado: **CURRENT DIRECTION / BASELINE 1.0**

## Decisión

La superficie previa al Manager será un **login real + consola simple de bootstrap**.

No es un bypass.

## Producción

```text
Entra ID
→ authenticated bootstrap access
→ Login/Bootstrap Console
```

No depende de que Users/Profile Projection ya exista.

## Local

Existe identity/provider local para desarrollo, pero:

```text
local != administrator automático
```

## Después de login

La consola muestra:

### Source histories

- releases/versions guardadas;
- current;
- historial disponible;
- integrity/state.

### Projections

- base projections;
- projected release/revision;
- project/reproject action;
- status.

### Derived resolutions

- readiness;
- blocked reason;
- dependency state.

### Infrastructure

- application resources;
- Cosmos/Storage status;
- mismatches;
- unavailable dependencies.

### Applications

- Manager readiness;
- ADA Generic readiness;
- Command Center readiness;
- Backend process readiness cuando pueda inferirse.

## Manager

Una vez que Users/Profile y requisitos Manager estén disponibles:

```text
Login/Bootstrap
→ Manager READY
→ normal Manager authorization
```

## No anonymous mutation

"Login previo al Manager" no significa acceso anónimo a project/configuration.

Las acciones de proyección/bootstrap requieren identidad/autorización explícita.
