# Manager — Source Blob Handoff

Estado: **DECIDED DIRECTION / NOT YET IMPLEMENTED**

Fuente:
`Atlanticus_ADA_Upgrade_Source_Blob_Trabajo_Pendiente.docx`
(10-09-2026)

## Cambio

Source productivo objetivo:

`SharePoint -> Azure Blob Storage`

Local sigue siendo equivalente de desarrollo/QA.

Projection sigue siendo:
- Cosmos DB;
- Local.

Power Automate sale del pipeline de persistencia/publicación de este Source.

## Historial

Cada publicación efectiva genera una versión lógica Atlanticus.

Cada versión:
- snapshot completo;
- autocontenida;
- inmutable;
- no depende de delta previo.

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

## No-op publish

Si workspace y current son funcionalmente equivalentes:
- no crear nueva versión sólo por presionar guardar.

La comparación puede apoyarse en content/manifest hash.

## Concurrencia

Frontend conserva detección y UX.

Backend debe garantizar ausencia de overwrite silencioso.

En Blob, ETag / conditional write es mecanismo natural candidato.

## Source -> Projection

Projection debe ejecutar un `source_release_id` concreto.

Nunca depender de un `latest` mutable durante la operación.

Projection debe poder declarar qué release representa.

## Pendiente contractual antes de implementar

- Release identity.
- Release granularity.
- Functional manifest.
- Physical layout.
- `SourceStore` contract.
- Backend concurrency preconditions.
- History/compare contract.
- Projection metadata.
- Retention requirements.

No implementar Blob Source antes de congelar estas fronteras.
