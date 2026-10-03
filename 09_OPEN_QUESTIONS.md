# Atlanticus — Open Questions

Estado: **CURRENT — COMMAND CENTER / ALARM BACKEND ANALYSIS NEXT**

## CLOSED

```text
Tool-scoped Source ownership
Global Users identity split
Tool User Membership
users-runtime
Tool Users Recovery Snapshot
Master Projection Users REPLACE

KPI named connections
KPI Registry materialization
Latest multi-Tool delivery
Historian rolling projection
Timeseries multi-Tool delivery
KPI History dataset representation boundary
```

## OPEN / NEXT — Command Center / Alarm

El siguiente chat debe resolver desde fuentes autoritativas, sin asumir respuesta previa:

```text
1. Qué responsabilidad pertenece hoy a ada-command-center.
2. Qué responsabilidad pertenece hoy a Alarm backend.
3. Qué piezas son producto/UI y cuáles son dominio/runtime reusable.
4. Si Alarm posee suficiente responsabilidad, ciclo de vida y contrato propio para madurar a engine.
5. Si Command Center debe consumir/orquestar Alarm en vez de poseer su núcleo.
6. Qué contratos deben quedar en backend antes de continuar consumidores Web.
7. Qué implementación actual debe mantenerse, moverse, reemplazarse o eliminarse.
```

No congelar una respuesta antes de inspeccionar `main`.

## BLOCKED

```text
KPI full operational E2E
```

Razón:

```text
Web requiere correcciones previas antes de poder levantar/configurar el entorno completo.
```

Lo que permanece UNVERIFIED:

```text
real Runtime -> Historian -> Timeseries execution
real multi-Tool Cosmos delivery
restart/recovery through the complete deployed flow
production Azure behavior
load/RU profile
```

## OPEN / SEPARATE

```text
Navigation PUBLIC / RESTRICTED
current-head artifact completeness
artifact installability
.env.detail exhaustive audit
system-derived/system-assigned values
distribution regeneration
ADA durable end-to-end
runtime restart/readback
recovery gate
macOS rcssmin
/health/ready checks
production Entra/Azure
Python migration
Collector / Time Status / UI
```
