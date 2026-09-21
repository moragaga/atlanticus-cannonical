# ADA Web — Infrastructure Startup

Estado: **CURRENT DIRECTION**

ADA Generic debe poder levantar su shell aun cuando:

- Backend no esté desplegado;
- no existan datos todavía;
- Cosmos/Storage opcional no esté configurado.

## Startup

```text
Web starts
→ framework infrastructure services
→ module services
→ resource/projection readiness when applicable
→ operational shell state
```

Atlanticus Web infrastructure includes runtime-owned `WebObservability`, exposed through the
frozen `ServiceRegistry`.

## Collector lifecycle

Collector does not poll during Web composition.

```text
/health/*
/assets/*
/.auth/*
→ do not start collector poller
```

The first real application request starts the worker-local polling thread.

Polling is independent from request execution; Cosmos is never read inline by the browser request
or browser refresh callback.

## Degraded data behavior

Collector source failure:

```text
→ request remains available
→ last good cache remains
→ incident is reported through WebObservability
```

Missing document:

```text
→ no forced error state
→ last good cache remains
```

Invalid contract:

```text
→ cache is not mutated
→ ERROR observability event
```

## Local / Azure infrastructure

Existing resource ownership rules remain unchanged.

Local may create/ensure resources only where current contracts authorize it.

Azure base infrastructure remains externally provisioned where defined.

Collector operational integration must reuse current Cosmos configuration/client ownership; it
must not add provisioning on every polling cycle.

## Next

```text
ADA-GENERIC-COLLECTOR-OPERATIONAL-INTEGRATION
PLANNED / NEXT
```

The next increment validates this lifecycle with the actual operational Tool/Cosmos composition.
