# Web Platform — Open Items

Estado: **OPEN**

## Users / Profiles / Navigation

Cerrados:

```text
USERS-STANDALONE-AUTHORITY-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-BOUNDARY-CUTOVER
CLOSED / VERIFIED / CURRENT
```

En progreso:

```text
PROFILES-CAPABILITY-EXTRACTION
IN PROGRESS
```

Siguiente único foco:

```text
USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER
PLANNED / NEXT
```

Pendientes posteriores:

1. eliminar `UsersProfilesConfiguration`;
2. eliminar `UsersProfilesAdministrationService`;
3. eliminar `UsersProfilesAdminDraft`;
4. separar Users Source y Profiles Source;
5. separar el payload Projection combinado;
6. migrar `UserConfiguration.profile_key` al contrato final `authority_key`;
7. remover `administrator` del boundary combinado sin alias hacia `root`;
8. definir y verificar composition Users + Profiles;
9. definir validación de referencias hacia functional profiles;
10. definir secuencia/recovery/audit para delete/reassign cuando exista referencia;
11. extraer Profiles UI de Users;
12. alinear Navigation => Profiles => Users;
13. verificar wiring real del selector local Jane/John.

La antigua frontera:

```text
USERS-PROFILES-ADMIN-COMPOSITION
```

queda `SUPERSEDED` por la secuencia incremental anterior.

## User Activity

14. Freeze `UserPageActivity` shape.
15. Freeze partition key.
16. Freeze deterministic ID strategy.
17. Verificar/aplicar `default_ttl_seconds=86400`.
18. Freeze semantics de active time y visit_count.
19. Freeze comportamiento de browser reload/client_session_id.
20. Definir dashboard query contract.

## Resource plan

21. Freeze `ApplicationResourcePlan`.
22. Freeze external/backend resource declaration.
23. Definir owner/required/optional semantics.
24. Definir named connection resolution.

## Local/cloud

25. Confirmar creación de database local dentro del lifecycle Web donde corresponda.
26. Confirmar permisos de creación/validación de containers en Azure.
27. Definir Storage provisioning parity.
28. Integrar resource preparation cuando sus contratos estén congelados.

## Readiness

29. Freeze READY/DEGRADED/ERROR semantics.
30. Definir dependencies required por aplicación.
31. Definir health/readiness endpoints/surface.

## Pre-Manager

32. Freeze route/name.
33. Freeze bootstrap authorization en Azure.
34. Eliminar `is_local` full-access bypass donde todavía exista.
35. Definir local development access sin recrear bypass.
36. Freeze projection action permissions.

## Projection orchestration

Cerrado:

```text
ProjectionTarget.dependencies
exact targets / deterministic normalization
```

Open:

37. definir retry/resume donde no esté cubierto por contracts CURRENT;
38. definir rollback/no-op sólo donde exista necesidad real;
39. mantener Source Release identity en consumers pendientes.

No crear coordinator global por defecto.

## Test contract cleanup

40. remover o reemplazar tests cuyo único objetivo sea inspeccionar ausencia de
    imports, archivos, funciones, clases o source tokens;
41. no crear validaciones automatizadas de CSS visual, responsive, spacing o branding;
42. conservar sólo checks de carga/existencia de assets cuando sean contractuales.

CURRENT conocido fuera de política:

```text
web/capabilities/users/core/tests/test_authority.py
test_users_core_has_no_profiles_dependency
```

Estado:

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN
```

No mezclarlo con el siguiente source ownership cutover.

## Deployment

43. crear esquema/ilustración final para soporte una vez congelado el contrato.

## Python baseline

44. alinear metadata `requires-python` con Python 3.14.7 en incremento separado;
45. qualificar globalmente `python:3.14.7-slim-trixie`.

No resolver silenciosamente dentro de Profiles extraction.
