# Manager — Source Blob Handoff

Estado: **SOURCE IMPLEMENTED / ROOT PROJECTION CUTOVER CLOSED / CONSUMER MIGRATION IN PROGRESS**

Fuente histórica:
`Atlanticus_ADA_Upgrade_Source_Blob_Trabajo_Pendiente.docx`
(10-09-2026)

## Cambio

Source productivo disponible:

```text
Azure Blob Storage
```

Local conserva semántica equivalente de desarrollo/QA.

Projection genérica usa handoff exact-release y cada dominio puede implementar:
- Cosmos DB;
- Local.

Navigation ya implementa ambos stores concretos.
Users ya implementa un ProjectionStore Cosmos canónico.

SharePoint y Power Automate permanecen únicamente donde todavía existen consumidores legacy.
Su retiro se realiza durante la migración del consumidor correspondiente, no como eliminación global anticipada.

## Estado Source

Implementado y validado:

```text
Source Core
Local Source
Blob Source
Projection exact-release Core
Manager root Projection exact-target transport
```

Source Core cubre:
- release identity;
- release granularity;
- functional manifest;
- `SourceStore`;
- concurrencia/current;
- History;
- exact release reads;
- integrity verification.

Blob cubre:
- manifest como commit point;
- create-only en first publish;
- conditional write por ETag;
- `ConcurrencyToken` público opaco e independiente del ETag;
- releases inmutables;
- orphan candidates fuera de History;
- recovery ante ACK ambiguo;
- restart e integridad.

## Historial

Cada publicación Source es:
- snapshot completo;
- autocontenida;
- inmutable;
- independiente de deltas previos.

`SourceReleaseId` identifica la publicación.

No equivale a `content_hash`.

Por tanto es válido:

```text
R1(content_hash = X)
R2(content_hash = X)
```

Blob Versioning NO es historial funcional.

Puede existir sólo como protección de infraestructura.

## Restore

Restaurar V1 no modifica V1 ni elimina versiones posteriores.

Ejemplo:
- current = V2;
- usuario toma V1 como base;
- publica;
- se crea V3;
- V1/V2 permanecen intactas.

Restore crea una publicación nueva.
Nunca repunta directamente current hacia una release histórica.

## No-op publish

La formulación histórica de no crear una versión cuando workspace y current son equivalentes queda refinada.

Contrato vigente:

```text
SourceStore.publish(PublishRequest válido)
→ crea una publicación Source nueva
```

Si un consumidor desea que una acción de UI sea no-op por equivalencia funcional, debe comparar antes de invocar `publish`.

El no-op por mismo contenido no pertenece al Source provider.

## Concurrencia

Frontend:
- puede detectar y explicar conflicto;
- deja la decisión humana.

Backend:
- aplica la precondición autoritativa mediante `ConcurrencyToken`.

No existe `force=True`.

En un overwrite autorizado:
- `basis_release` conserva la base original del trabajo;
- el consumidor relee Source current;
- usa el `ConcurrencyToken` fresco;
- el provider Source sigue aplicando CAS.

ETag es un detalle técnico interno del provider Blob y no el contrato público del consumidor.

## Source -> Projection

Projection ejecuta un target exacto:

```text
ProjectionTarget =
    SourceKey
    +
    SourceReleaseRef
```

`project(target)`:
- resuelve la release exacta mediante `read_release`;
- no consulta Source current durante la operación;
- persiste provenance con `source_release_id` en los stores canónicos que lo implementan.

Source puede avanzar mientras se proyecta una release anterior.
La proyección seleccionada sigue siendo válida y puede quedar `OUTDATED` al comparar después.

Un fallo de Projection:
- no revierte Source;
- no reemplaza el último active projection exitoso.

Retry conserva el mismo target sin republicar Source.

## Manager root Projection checkpoint

`MANAGER-ROOT-CANONICAL-CUTOVER` quedó `CLOSED / VERIFIED / CURRENT` en:

```text
moragaga/atlanticus@5fd2858c4bd19c8f9cc416e0996162cb7a3f8c06
```

Contrato implementado:
- `ConfigurationLifecycleWorkflow.get_current_projection_target()` expone `ProjectionTarget | None`;
- `ConfigurationLifecycleWorkflow.project(...)` recibe `ProjectionTarget`;
- `ProjectionExecutionResult` conserva `target: ProjectionTarget`;
- `ManagerProjectionCoordinator.project(...)` transporta el target intacto y no relee current;
- callback Project selecciona current server-side inmediatamente antes de ejecutar;
- browser state no provee la identidad ejecutable;
- projection signal expone `source_key`, `source_release_id`, `source_published_at_utc` y `projection_revision`;
- un target histórico explícito puede ejecutarse sin ser reemplazado por current.

Este cierre no elimina todos los `source_revision: str` del Manager. Publicación/verificación/history/workspace legacy permanecen fuera del hito.

## Navigation checkpoint

Navigation Configuration ya implementa:
- Source codec/service sobre Source Core;
- History real de Source;
- exact release reads;
- Projection builder;
- ProjectionStore Local;
- ProjectionStore Cosmos;
- runtime consumiendo ProjectionStore canónico.

Estado posterior al root cutover:
- consumer administrativo Navigation: **PLANNED**;
- eliminación de Source/Projection legacy Navigation: **BLOCKED** hasta validar que no quedan consumidores legacy.

No existe evidencia en `atlanticus:main@5fd2858...` de una clase productiva llamada `NavigationManagerWorkflowAdapter`; canonical no debe asumir ese nombre como implementación existente.

No crear adaptadores string/release temporales para completar la migración.

## Users checkpoint

Users Configuration ya implementa:
- Source codec/service sobre Source Core;
- History y exact release reads;
- `UsersProjectionBuilder`;
- `SourceProjectionService` con target `SourceKey + SourceReleaseRef`;
- `ProjectionStore[UsersConfigurationCatalog]` sobre Cosmos;
- provenance canónico con `source_release_id`;
- create-only para primer active;
- ETag/CAS para reemplazo;
- retry same-target idempotente;
- conflicto concurrente different-target explícito.

Estado:

```text
USERS-CANONICAL-SOURCE-1      CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-PROJECTION-2  CLOSED / VERIFIED / CURRENT
MANAGER-ROOT-CANONICAL-CUTOVER CLOSED / VERIFIED / CURRENT
```

Checkpoint root cutover:

```text
moragaga/atlanticus@5fd2858c4bd19c8f9cc416e0996162cb7a3f8c06
```

Continúan abiertos:
- migración administrativa de Users: **PLANNED**;
- migración de `projection_source_revision` legacy dentro de `users.runtime`: **PLANNED** y ya no bloqueada por el root Projection cutover;
- eliminación de Source/Projection legacy Users: **BLOCKED** hasta validar consumidores;
- resource topology/provisioning físico del canonical Users Projection store: **PLANNED / OPEN**.

No existe evidencia en `atlanticus:main@5fd2858...` de una clase productiva llamada `UsersManagerWorkflowAdapter`; canonical no debe asumir ese nombre como implementación existente.

El cierre canónico de Projection no crea equivalencia entre `UsersConfigurationBundle.revision` y `SourceReleaseId`.

No crear adaptadores string/release temporales ni un segundo coordinator Manager.

## Pendiente

Fuera de Source/Projection Core y del root Projection cutover ya cerrados:
- retention/cleanup policy;
- provenance exact-release de `users.runtime`;
- migración de consumidores administrativos Navigation y Users;
- browser WORKSPACE/IndexedDB del Manager;
- retiro efectivo de SharePoint/Power Automate donde deje de existir consumidor;
- eliminación de adapters/contracts legacy de dominio sólo después de validar consumidores;
- orchestration multi-capability cuando exista requisito real.
