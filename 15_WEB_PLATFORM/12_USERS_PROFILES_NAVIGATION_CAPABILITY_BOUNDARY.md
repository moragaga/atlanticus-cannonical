# Web Platform — Users / Profiles / Access / Navigation / Manager Capability Boundary

Estado: **CURRENT / MANAGER AUTHORIZATION REFINED 2026-10-01**

Implementation checkpoint:

```text
moragaga/atlanticus@6fd1512afed73e76f7c344f3acb989b601c453e3
```

## Propósito y ownership

```text
Atlanticus Global Users         identity + lifecycle + user → profile_key
Atlanticus Generic Profiles    definition + catalog + Source + Projection
ADA Access                      profile_key → operational access_keys
Atlanticus Generic Navigation  route structure + profile visibility
Atlanticus Manager             administrative shell + administrative authorization
ADA Generic                     explicit composition/consumer
```

Atlanticus Core no depende de ADA.

## Users / Profiles

System profiles continúan:

```text
basic
root
guest
local
```

Users conserva `profile_key`.
Profiles conserva catálogo y definición.
Este hito no cambia sus persistencias ni workflows.

## ADA Access CURRENT

```text
profile_key → access_keys
```

Es un contrato ADA específico para accesos operacionales.

No se proyecta a `ManagerPrincipal.access_keys`.

No crear persistencia `user → Manager access_keys` para reemplazar esta separación.

## Manager authorization CURRENT

Contrato genérico:

```text
access_key is None                      → DENY
principal.administrative_override       → ALLOW
access_key in principal.access_keys     → ALLOW
otherwise                               → DENY
```

Manager Core no infiere override desde profiles ni `is_local`.

La composición ADA decide:

```text
managed root                         → administrative_override
trusted local + local environment    → administrative_override
basic / guest / custom               → no Manager administration
bootstrap root                       → no implicit Manager administration
```

Granular `access_keys` se conserva para administración delegada futura.

## Navigation CURRENT

Navigation mantiene su propia autorización de rutas y visibilidad.

Navigation no obtiene permisos Manager desde ADA Access.

El uso de un contexto administrativo puede producir comportamiento especial de Navigation según su contrato propio, pero esa semántica no reemplaza la policy del Manager.

## Refinamiento respecto de canonical anterior

La formulación histórica:

```text
administrative_override is only a Navigation recovery exception
and does not grant Manager access
```

queda **SUPERSEDED** por implementation CURRENT.

`ManagerPrincipal.administrative_override` participa ahora directamente en `manager_access_granted()`.

Esto no fusiona Navigation y Manager: siguen siendo capabilities independientes con policies distintas.

## Local runtime

El principal local temporal usa:

```text
profile_keys=('local',)
access_keys=()
administrative_override=True
is_local=True
```

No mantener una enumeración exhaustiva `*.manage`.

## Qualification

```text
ADA Generic application       288 passed
ADA Generic ruff              PASS
ADA Configuration Manager      70 passed
Atlanticus Manager Core        85 passed
MANAGER_ACCESS_KEYS search      0 matches
```

## Reglas congeladas

```text
Atlanticus generic core → ADA dependency          FORBIDDEN
Users → profile_key                               CURRENT
ADA Access ownership                              ADA-SPECIFIC
ADA Access → Manager permissions                  FORBIDDEN
Manager override inference inside Core            FORBIDDEN
is_local alone → Manager administration           FORBIDDEN
root profile alone inside Core → administration   FORBIDDEN
composition-owned override decision               CURRENT
granular Manager access_keys                      CURRENT
access_key=None                                   DENY
legacy shims / aliases / duplicate contracts      FORBIDDEN
```

## OPEN separado

- Production Entra and physical identity qualification.
- Durable login/bootstrap-data E2E.
- Global CI/Ruff.
- Tooling contract review and product Golden Path.
