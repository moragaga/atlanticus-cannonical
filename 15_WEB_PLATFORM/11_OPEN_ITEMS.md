# Web Platform — Open Items

Estado: **OPEN**

Los items cerrados no deben reabrirse para restaurar simetría o legacy.

## Closed baselines relevantes

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-PERSISTED-DATA-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CAPABILITY-EXTRACTION
CLOSED / VERIFIED / CURRENT

PROFILES-INDEPENDENT-SOURCE-LIFECYCLE
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROFILES-CONFIGURATION
CLOSED / VERIFIED / CURRENT

NONPROMOTED-ACCESS-SEMANTICS-CORRECTION
CLOSED / VERIFIED / CURRENT

NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
CLOSED / VERIFIED / CURRENT
```

Ya no son open items:

```text
Users Source/Projection lifecycle
UsersProfilesConfiguration
pending/resolved runtime dual schema
Profiles extraction/source lifecycle
NavigationProfileOption
_BASE_PROFILES
NavigationProfileOptionsProvider
Navigation -> Users profile options
Navigation -> ADA Access authorization dependency
Navigation Configuration -> Profiles catalog alignment
```

## Manager authorization — PROPOSED NEXT

Open:

1. revisar `ManagerPrincipal` CURRENT;
2. revisar `ManagerModuleAccess` CURRENT;
3. revisar `DefaultManagerAuthorizationPolicy` CURRENT;
4. determinar si `is_local` sigue siendo bypass válido o debe convertirse en permisos explícitos;
5. eliminar o justificar `administrator` dentro de Manager sin mapping hacia `root`;
6. revisar helpers `_can_manage_navigation/_tools/_kpis` de ADA Configuration Manager;
7. verificar composition local antes de remover bypass;
8. definir tests de comportamiento final sin congelar implementación interna.

Estado:

```text
Manager authorization stale administrator/local semantics
OPEN / PROPOSED NEXT
```

## Navigation runtime fallback

9. resolver composition exacta para identidad autenticada sin promoted `UserRecord`;
10. definir `NavigationPrincipal` efectivo sin crear UserRecord ficticio;
11. usar `guest` sólo como profile normal si existe en `ProfileCatalog`;
12. no crear dependency Navigation -> Users/ADA Access.

Estado:

```text
OPEN / SEPARATE
```

## Users Administration

```text
USERS-ADMINISTRATION-SURFACE-CUTOVER
PLANNED / SEPARATE
```

Open:

13. UI/surface para lifecycle actual de Users;
14. explicit promote/update/repair operations según contratos existentes;
15. concrete Directory provider wiring cuando exista evidencia suficiente.

No reintroducir Users Source/Projection.

## Entra / Directory

16. localizar provider existente si existe;
17. si no existe, diseñar sólo desde configuración/credenciales reales;
18. no inventar Graph scopes, tenant ids, credential type ni endpoints.

Estado:

```text
UNVERIFIED
```

## ADA Access runtime

19. verificar wiring runtime real;
20. no convertir ADA Access en dependency de Navigation;
21. no mover Access application-specific a Atlanticus generic sin evidencia de reutilización.

Estado:

```text
OPEN / SEPARATE
```

## User Activity

22. Freeze `UserPageActivity` shape.
23. Freeze partition key.
24. Freeze deterministic ID strategy.
25. Verificar/aplicar `default_ttl_seconds=86400`.
26. Freeze semantics de active time y visit_count.
27. Freeze comportamiento de browser reload/client_session_id.
28. Definir dashboard query contract.

## Resource plan

29. Freeze `ApplicationResourcePlan` cuando corresponda.
30. Freeze external/backend resource declaration.
31. Definir owner/required/optional semantics.
32. Definir named connection resolution.

## Readiness

33. Freeze READY/DEGRADED/ERROR semantics.
34. Definir dependencies required por aplicación.
35. Definir health/readiness endpoints/surface.

## Test contract cleanup

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN
```

36. remover/reemplazar tests que congelen implementación interna sin comportamiento;
37. revisar findings preexistentes en `test_web_contract.py` y `test_web_source_contract.py`;
38. no crear validaciones automatizadas de CSS visual/responsive/spacing/branding;
39. conservar checks de assets sólo cuando su carga sea contractual.

## Python baseline

40. alinear metadata `requires-python` con Python 3.14.7 en incremento separado;
41. qualificar globalmente `python:3.14.7-slim-trixie`.

## CI / lint transversal

42. qualificar CI remoto para un checkpoint CURRENT cuando exista evidence;
43. tratar Ruff global preexistente en incremento separado.
