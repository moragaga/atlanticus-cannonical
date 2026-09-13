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

## Users runtime projection

19. `USERS-RUNTIME-PROJECTION-BOUNDARY`: auditar `UsersProjectionRepository` y adapters actuales para definir ownership del write Managed hacia `users.runtime`.
20. Freeze documento/operación durable para materializar `ResolvedUserRecord` sin duplicar la autoridad Source.
21. Freeze transición Pending→Resolved y concurrencia contra `observe()` create-only.
22. Freeze comportamiento cuando un usuario desaparece de configuración: delete, disable, preserve u otra semántica explícita.
23. Freeze idempotencia/reprojection y provenance/audit.
24. Determinar package/provider boundary del writer antes de implementarlo; no agregar `upsert_resolved()` al runtime store por conveniencia.

## Readiness

25. Freeze READY/DEGRADED/ERROR semantics.
26. Definir qué dependencies son required por aplicación.
27. Definir health/readiness endpoints/surface.

## Pre-Manager

28. Freeze route/name.
29. Freeze bootstrap authorization en Azure.
30. Eliminar `is_local` full-access bypass.
31. Definir local development access sin volver a crear bypass.
32. Freeze projection action permissions.

## Projection orchestration

33. Freeze projection dependency contract.
34. Freeze deterministic ordering.
35. Definir retry/resume.
36. Definir rollback/no-op semantics.
37. Integrar Source Release identity.

## Deployment

38. Crear esquema/ilustración final para soporte una vez congelado el contrato.
