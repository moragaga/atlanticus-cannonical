# Source Storage — Concurrency

Estado: **CURRENT / FROZEN CORE+LOCAL**

## Principio

La concurrencia autoritativa pertenece al provider Source que promueve current.

La UI/Manager puede detectar y explicar un conflicto, pero no sustituye la precondición durable.

## ConcurrencyToken

El contrato público usa:

```text
ConcurrencyToken
```

Es opaco.

Los consumidores sólo pueden:
- conservarlo junto al snapshot observado;
- devolver exactamente ese token al publicar;
- comparar identidad opaca cuando corresponda.

No conocen:
- ETag;
- mtime;
- inode;
- filesystem generation;
- provider-specific revision.

## SourceSnapshot

Un snapshot publicado contiene juntos:
- `current`;
- `concurrency_token`.

Un Source vacío contiene:
- `current = None`;
- `concurrency_token = None`.

No se permite separar conceptualmente current del token exacto observado con él.

## PublishRequest

La publicación incluye:

```text
expected_concurrency_token
```

La promoción sólo puede ocurrir si el provider sigue observando exactamente esa versión de current.

No existe bypass `force=True`.

Si se desea sobrescribir sobre una versión más nueva:

```text
reread current
→ decide
→ publish against fresh token
```

## First publish

La primera publicación utiliza el mismo principio:

```text
expected_concurrency_token = None
```

Sólo un candidato puede crear/promover el primer current.

Publicadores concurrentes restantes reciben `SourceConcurrencyError`.

## Local

Local implementa CAS real entre procesos:

```text
materialize candidate
→ acquire Source lock
→ reread manifest/current
→ compare exact expected token
→ atomic manifest replace
```

El lock protege la operación compare-and-promote.

`os.replace` por sí solo no se considera CAS.

El token Local se deriva del contenido exacto del manifest y sigue siendo opaco fuera del provider.

## Candidate perdido

Si un candidato se materializa pero pierde la promoción:
- permanece orphan;
- no es current;
- no entra a History;
- no se elimina sin una política de cleanup separada.

## Blob

Blob deberá mapear internamente este contrato a conditional writes.

ETag es el mecanismo natural esperado, pero la decisión concreta se valida en 1A.2 contra `connectivity/storage` y Azure Blob.

No filtrar ETag a Manager ni a consumidores.

## ACK ambiguo / recovery

Para providers remotos, una respuesta ambigua después del conditional write debe resolverse releyendo manifest:

```text
candidate == current
→ published

expected current unchanged
→ not promoted

different current
→ conflict / another winner
```

No republicar automáticamente sólo porque se perdió un ACK.

## Manager

Manager representa como mínimo:

```text
BASE
SOURCE
WORKSPACE
PROJECTION
```

La decisión humana de conflicto es separada de la garantía durable.

No hay merge automático inicial.
