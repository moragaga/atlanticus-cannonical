# ADA Web — Infrastructure Startup

Estado: **CURRENT DIRECTION**

ADA Generic debe poder levantar su shell aun cuando:

- Backend no esté desplegado;
- no existan datos todavía;
- Cosmos/Storage opcional no esté configurado.

## Startup

```text
Web starts
→ resource plan
→ prepare/validate resources
→ projection readiness
→ operational shell state
```

## Local

Puede crear la Cosmos database local y asegurar containers.

## Azure

La DB base preexiste.

La Web valida y prepara application containers permitidos.

## Error

Un container con partition key o TTL incorrecto:

```text
→ resource ERROR
→ dependent capability unavailable
→ system/bootstrap surface remains reachable
```

## Backend

ADA backend se habilita después del Web bootstrap.

No debe ejecutar provisioning por ciclo.
