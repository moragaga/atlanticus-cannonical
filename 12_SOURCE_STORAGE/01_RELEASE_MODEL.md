# Source Storage — Release Model

Estado: **CURRENT / FROZEN FOR 1A.1**

Implementación de referencia:

```text
web/capabilities/source/core
web/capabilities/source/local
```

## Identidades

`SourceKey` es una identidad lógica de Source.

No representa filesystem path, Blob path, container ni URL.

`SourceReleaseId` es una identidad opaca de publicación.

No es content-addressed y no equivale a `content_hash`.

Dos publicaciones distintas pueden compartir el mismo `content_hash`.

`SourceReleaseRef` contiene:

```text
release_id
published_at_utc
```

La referencia incluye el timestamp porque los providers pueden usarlo para resolver particiones físicas sin introducir un índice adicional.

## Release

Cada publicación efectiva crea una release:
- completa;
- inmutable;
- auditable;
- independiente de Projection.

Una release contiene metadata durable y recursos lógicos.

Metadata implementada:

```text
schema_version
source_key
release_ref
content_hash
resources[]
previous_published_release
basis_release
```

Cada recurso registra:

```text
logical_path
byte_length
digest
```

No se congeló `media_type` en 1A.1.

## Draft

Guardar un draft de dominio no crea necesariamente una release Source.

Esta decisión pertenece al consumidor/Manager.

`SourceStore.publish()` sí crea una nueva publicación cuando es invocado correctamente, incluso si el contenido tiene el mismo hash que current.

La política de no-op por mismo contenido no pertenece al core Source.

## Publish

Protocolo funcional:

```text
read current snapshot
→ validate exact expected concurrency token
→ assign release identity/reference
→ materialize immutable candidate
→ verify candidate integrity
→ build manifest
→ revalidate exact concurrency token
→ atomically promote manifest
→ return new current snapshot
```

La materialización del candidato no equivale a publicación.

Sólo la promoción del manifest convierte el candidato en current publicado.

## Manifest funcional

`manifest.json` es el único commit point de publicación.

Representa:

```text
schema_version
source_key
current:
  release_ref:
    release_id
    published_at_utc
  content_hash
```

El token de concurrencia viaja separado del JSON del manifest.

No existe:
- `published.json`;
- copia mutable `current/configuration.json`;
- segundo puntero durable de publicación.

## History

History funcional sigue exclusivamente la cadena:

```text
current
→ previous_published_release
→ previous_published_release
→ ...
```

Un candidate materializado pero no promovido no pertenece a History.

No se usa directory listing como autoridad funcional.

No existe un segundo índice durable de History en 1A.1.

## Restore / republish

Restaurar contenido histórico significa:

```text
read historical release
→ load/validate workspace
→ read current
→ ordinary publish
→ new release
```

Nunca se repunta directamente current a una release histórica.

`basis_release` registra la base/provenance del contenido.

`previous_published_release` registra el predecessor real de la publicación.

Ambos conceptos son independientes.

## Integridad

El contenido posee:
- digest por recurso;
- content hash agregado.

La integridad puede detectar:
- metadata inválida;
- recurso faltante;
- tamaño distinto;
- digest distinto;
- content hash distinto.

Un fallo de integridad no promueve ni modifica current.

## Nota histórica

`backend/configuration/manifest.py` representa otra responsabilidad y no es el functional Source Release Manifest.

No reutilizarlo por homonimia.
