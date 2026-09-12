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

10. Freeze `ApplicationResourcePlan`.
11. Freeze external/backend resource declaration.
12. Definir owner/required/optional semantics.
13. Definir named connection resolution.

## Local/cloud

14. Confirmar creación de database sólo local.
15. Confirmar qué containers puede crear Web en Azure según permisos.
16. Definir Storage provisioning parity.

## Readiness

17. Freeze READY/DEGRADED/ERROR semantics.
18. Definir qué dependencies son required por aplicación.
19. Definir health/readiness endpoints/surface.

## Pre-Manager

20. Freeze route/name.
21. Freeze bootstrap authorization en Azure.
22. Eliminar `is_local` full-access bypass.
23. Definir local development access sin volver a crear bypass.
24. Freeze projection action permissions.

## Projection orchestration

25. Freeze projection dependency contract.
26. Freeze deterministic ordering.
27. Definir retry/resume.
28. Definir rollback/no-op semantics.
29. Integrar Source Release identity.

## Deployment

30. Crear esquema/ilustración final para soporte una vez congelado el contrato.
