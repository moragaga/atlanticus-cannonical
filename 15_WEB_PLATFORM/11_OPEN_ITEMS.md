# Web Platform — Open Items

Estado: **OPEN**

## Capability composition

1. Freeze contrato de optional bindings.
2. Extraer Users→Navigation coupling de ADA Manager.
3. Confirmar qué integrations necesitan packages explícitos y cuáles basta inyectar por provider.

## User Activity

4. Freeze `UserPageActivity` shape.
5. Freeze partition key.
6. Freeze deterministic ID strategy: application + actor + session + page.
7. Verificar/aplicar `default_ttl_seconds=86400`.
8. Freeze semantics de active time y visit_count.
9. Freeze comportamiento de browser reload/client_session_id.
10. Definir dashboard query contract.

## Resource plan

11. Freeze `ApplicationResourcePlan`.
12. Freeze external/backend resource declaration.
13. Definir owner/required/optional semantics.
14. Definir named connection resolution.

## Local/cloud

15. Confirmar creación de database sólo local dentro del lifecycle Web; el bridge Cosmos no crea database.
16. Confirmar qué containers puede crear Web en Azure según permisos.
17. Definir Storage provisioning parity.
18. Integrar el bridge Cosmos ya cerrado al punto de resource preparation cuando los contratos anteriores estén congelados.

## Users Source / Projection / Profiles

Cerrados:

```text
USERS-RUNTIME-PROJECTION-BOUNDARY  CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-SOURCE-1           CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-PROJECTION-2       CLOSED / VERIFIED / CURRENT
USERS-CONTRACT-SEPARATION          CLOSED / VERIFIED / CURRENT
UCS-1 CANONICAL-CONTRACT-SPLIT     CLOSED / VERIFIED / CURRENT
```

Ya no están OPEN:
- ownership durable Profiles vs Users;
- destino canónico de `administrator_*`;
- destino canónico de `guest_*`;
- payload Projection que reemplaza `UsersConfigurationCatalog`;
- orphan validation;
- necesidad de una Projection independiente de Profiles para este baseline.

Permanecen OPEN/BLOCKED:

19. `USERS-PROFILES-ADMIN-COMPOSITION`.
20. Migrar `UsersAdministrationService` y authoring/drafts a los contratos separados.
21. Definir UX delete/reassign de Profiles referenciados respetando no-orphans.
22. `USERS-RUNTIME-CANONICAL-CUTOVER`.
23. `USERS-RUNTIME-EXACT-RELEASE-PROVENANCE`, sin shim `SourceReleaseId <-> str`.
24. Freeze resource topology/provisioning del Cosmos canonical Projection store si deployment lo requiere.
25. Legacy Users Source/Projection/admin deletion sólo tras consumer migration.
26. Validar composition root productivo del canonical Projection store en su incremento de integración.
27. Definir destino final de `FileUsersProjectionProfileCatalog` durante consumer cutover.

## Readiness

28. Freeze READY/DEGRADED/ERROR semantics.
29. Definir qué dependencies son required por aplicación.
30. Definir health/readiness endpoints/surface.

## Pre-Manager

31. Freeze route/name.
32. Freeze bootstrap authorization en Azure.
33. Eliminar `is_local` full-access bypass.
34. Definir local development access sin volver a crear bypass.
35. Freeze projection action permissions.

## Projection orchestration

36. Freeze projection dependency contract sólo si múltiples projections realmente lo requieren.
37. Freeze deterministic ordering cuando exista esa necesidad.
38. Definir retry/resume.
39. Definir rollback/no-op semantics.
40. Mantener Source Release identity en consumers pendientes.

UCS-1 no crea un segundo coordinator ni un release clock independiente de Profiles.

## Deployment

41. Crear esquema/ilustración final para soporte una vez congelado el contrato.
