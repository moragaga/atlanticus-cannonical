# Manager — Source Blob Handoff

Estado: **SOURCE IMPLEMENTED / CONSUMER MIGRATION IN PROGRESS**

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

SharePoint y Power Automate permanecen únicamente donde todavía existen consumidores legacy.
Su retiro se realiza durante la migración del consumidor correspondiente, no como eliminación global anticipada.

## Estado Source

Implementado y validado:

```text
Source Core
Local Source
Blob Source
Projection exact-release Core
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
- persiste provenance con `source_release_id`.

Source puede avanzar mientras se proyecta una release anterior.
La proyección seleccionada sigue siendo válida y puede quedar `OUTDATED` al comparar después.

Un fallo de Projection:
- no revierte Source;
- no reemplaza el último active projection exitoso.

Retry conserva el mismo target sin republicar Source.

## Navigation checkpoint

Navigation Configuration ya implementa:
- Source codec/service sobre Source Core;
- History real de Source;
- exact release reads;
- Projection builder;
- ProjectionStore Local;
- ProjectionStore Cosmos;
- runtime consumiendo ProjectionStore canónico.

Permanece bloqueado:
- consumer administrativo Manager;
- eliminación de Source/Projection legacy Navigation.

Razón:
el coordinator Manager y `NavigationManagerWorkflowAdapter` productivos todavía usan `source_revision: str`.

No crear adaptadores string/release temporales para ocultar este bloqueo.

## Pendiente

Fuera del Source Core ya cerrado:
- retention/cleanup policy;
- migración de consumidores restantes;
- Manager root canonical cutover;
- retiro efectivo de SharePoint/Power Automate donde deje de existir consumidor;
- eliminación de adapters legacy de dominio sólo después de validar consumidores.
