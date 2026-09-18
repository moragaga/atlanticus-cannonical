# Web Platform — Open Items

Estado: **OPEN**

## Global Users

Cerrado:

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Ya no son open items:

```text
separate Users Source
UsersConfiguration
UsersProfilesConfiguration
UsersProfilesAdministrationService
UsersProfilesAdminDraft
Users generic projection
Users Manager module
pending/resolved runtime dual schema
login observe pending
```

## Users persisted data — NEXT

```text
USERS-PERSISTED-DATA-CUTOVER
PLANNED / NEXT
```

Open items de este foco:

1. inspeccionar stores/topology reales antes de diseñar migración;
2. inventariar documentos Cosmos legacy/current por schema real;
3. confirmar si existe registry Blob CURRENT y su ubicación/configuración real;
4. separar datos global Users de información Profiles que deba preservarse;
5. congelar transformación one-shot hacia `atlanticus_users_registry` schema 1;
6. congelar transformación one-shot hacia `atlanticus_user` schema 1;
7. definir verification de strong identity y Blob/Cosmos parity;
8. definir comportamiento de retry si Registry existe y Cosmos todavía no;
9. definir condición verificable para borrar legacy persisted data;
10. no crear runtime adapters, shims ni old-schema readers.

## Users Administration

```text
USERS-ADMINISTRATION-SURFACE-CUTOVER
PLANNED / AFTER PERSISTED DATA
```

Open:

11. UI/surface para PROMOTABLE / CONFLICT / PROMOTED;
12. explicit promote/update commands;
13. repair/recovery operations para inconsistencias detectadas;
14. audit semantics cuando un operation contract real lo requiera;
15. concrete Directory provider wiring.

No reintroducir Users Source/Projection.

## Entra / Directory

16. localizar provider existente si existe;
17. si no existe, diseñar sólo desde configuración/credenciales reales;
18. no inventar Graph scopes, tenant ids, credential type ni endpoints.

Estado:

```text
UNVERIFIED
```

## Profiles / Access

```text
PROFILES-CAPABILITY-EXTRACTION
IN PROGRESS

PROFILES-INDEPENDENT-SOURCE-LIFECYCLE
PLANNED

ACCESS-PROFILES-CONFIGURATION
PLANNED
```

Open:

19. lifecycle independiente de Profiles;
20. Profiles admin/UI owner;
21. exact Global User -> app-specific Profile association owner;
22. Access configuration contract;
23. delete/reassign/recovery semantics sólo cuando referencias reales estén congeladas.

No abrir estos puntos dentro del próximo Users persisted-data increment.

## Navigation

24. alinear Navigation con effective Profile/Access result después de congelar esos contracts;
25. no crear dependencia directa a Users por analogía.

## User Activity

26. Freeze `UserPageActivity` shape.
27. Freeze partition key.
28. Freeze deterministic ID strategy.
29. Verificar/aplicar `default_ttl_seconds=86400`.
30. Freeze semantics de active time y visit_count.
31. Freeze comportamiento de browser reload/client_session_id.
32. Definir dashboard query contract.

## Resource plan

33. Freeze `ApplicationResourcePlan` cuando corresponda.
34. Freeze external/backend resource declaration.
35. Definir owner/required/optional semantics.
36. Definir named connection resolution.

## Readiness

37. Freeze READY/DEGRADED/ERROR semantics.
38. Definir dependencies required por aplicación.
39. Definir health/readiness endpoints/surface.

## Test contract cleanup

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED
```

40. remover/reemplazar tests que congelen implementación interna sin comportamiento;
41. no crear validaciones automatizadas de CSS visual/responsive/spacing/branding;
42. conservar checks de assets sólo cuando su carga sea contractual.

## Python baseline

43. alinear metadata `requires-python` con Python 3.14.7 en incremento separado;
44. qualificar globalmente `python:3.14.7-slim-trixie`.

## CI / lint transversal

45. qualificar CI remoto cuando exista workflow/status evidence;
46. tratar Ruff global preexistente en incremento separado, no durante Users data cutover.
