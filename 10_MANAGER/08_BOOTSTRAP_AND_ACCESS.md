# Manager — Bootstrap and Access

Estado: **CURRENT — MANAGER AUTHORIZATION CONVERGED / ADA ACCESS DECOUPLED**

Implementation checkpoint:

```text
moragaga/atlanticus@6fd1512afed73e76f7c344f3acb989b601c453e3
```

Historical decisions inspected:

```text
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

## Manager e Identity CURRENT

```text
BOOTSTRAP ACCESS != MANAGER ACCESS
ADA ACCESS != MANAGER ACCESS
```

La autenticación crea contexto de identidad. No concede por sí misma administración Manager.

`ManagerPrincipal` mantiene:

```text
subject_id
display_name
profile_keys
access_keys
is_local
administrative_override
```

La autorización genérica CURRENT es:

```text
access_key is None                      → DENY
administrative_override=True            → ALLOW
access_key in principal.access_keys     → ALLOW
otherwise                               → DENY
```

`DefaultManagerAuthorizationPolicy` delega esta decisión a `manager_access_granted()`.

La misma regla es consumida por los `can_manage` de ADA Configuration Manager que antes consultaban `principal.access_keys` directamente.

Manager Core no importa ni interpreta ADA Access, Profiles, Users o Navigation para decidir override.

## Composición ADA CURRENT

ADA Generic resuelve el principal administrativo desde `AccessSnapshot` + `EffectiveUser`.

```text
managed root
→ administrative_override=True

trusted local identity
+ local environment
→ administrative_override=True
→ is_local=True

basic / guest / custom
→ administrative_override=False

bootstrap root without managed user
→ administrative_override=False
```

Un usuario deshabilitado se rechaza.
La identidad autenticada y el `EffectiveUser` deben referir al mismo subject.

La ruta trusted-local requiere identidad local canónica y entorno local.
`is_local=True` por sí solo no concede administración.

## ADA Access queda separado

La composición Manager ya no recibe:

```text
AdaAccessConfiguration
ProfileCatalog
```

para calcular permisos Manager.

ADA Access conserva su contrato operacional:

```text
profile_key → access_keys
```

pero esos `access_keys` no se proyectan a `ManagerPrincipal.access_keys`.

## Runtime local

El principal temporal del Configuration Manager local usa:

```text
profile_keys=('local',)
access_keys=()
administrative_override=True
is_local=True
```

No existe una lista exhaustiva de permisos para simular full access.

`MANAGER_ACCESS_KEYS` fue eliminado del código revisado.

## Users y Master — fronteras conservadas

Users continúa con su ownership y recovery propios.
Master externo continúa siendo una superficie separada y no se convierte en `ManagerPrincipal`.

Este hito no cambia los contratos de Users recovery, Master material, preview/apply ni sus controles.

## Qualification del hito

```text
ADA Generic application       288 passed
ADA Generic ruff              PASS
ADA Configuration Manager      70 passed
Atlanticus Manager Core        85 passed
MANAGER_ACCESS_KEYS search      0 matches
git diff --check               PASS
```

No convertir estas pruebas en una qualification global del monorepo ni en evidencia Entra/Azure.

## OPEN separado

```text
production Entra
physical login/bootstrap data E2E
durable restart/recovery E2E
multiworker qualification
global workspace CI/Ruff
```

No reabrir autorización Manager para resolver estos frentes.
