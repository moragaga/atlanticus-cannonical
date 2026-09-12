# Source Storage — SourceStore Contract

Estado: **CURRENT / FROZEN FOR 1A.1**

## Ownership

Source es una capability genérica Web:

```text
web/capabilities/source/
```

No pertenece a backend jobs.

Connectivity Storage es una dependencia técnica posible de Blob y no debe absorber la semántica Source.

## Packages actuales

```text
atlanticus-web-source==0.1.0
atlanticus-web-source-local==0.1.0
```

## Objetivo

Local y Azure Blob deben exponer la misma semántica funcional al resto del sistema.

La infraestructura cambia; el contrato funcional no.

## SourceStore

Contrato implementado:

```python
get_current(source_key: SourceKey) -> SourceSnapshot

read_release(
    source_key: SourceKey,
    release_ref: SourceReleaseRef,
) -> tuple[SourceReleaseMetadata, tuple[SourceResource, ...]]

publish(request: PublishRequest) -> PublishResult

query_history(query: HistoryQuery) -> HistoryPage

verify_release(
    source_key: SourceKey,
    release_ref: SourceReleaseRef,
) -> IntegrityResult
```

`read_release` y `verify_release` reciben `SourceReleaseRef`, no sólo `release_id`.

Esto permite a un provider resolver una release con partición temporal sin escanear todo el storage ni introducir un índice durable prematuro.

## Modelos públicos

El contrato incluye:
- `SourceKey`;
- `SourceReleaseId`;
- `SourceReleaseRef`;
- `Digest`;
- `SourceResource`;
- `SourceResourceMetadata`;
- `SourceReleaseSummary`;
- `SourceReleaseMetadata`;
- `SourceManifest`;
- `ConcurrencyToken`;
- `SourceSnapshot`;
- `PublishRequest`;
- `PublishResult`;
- `HistoryQuery`;
- `HistoryPage`;
- `IntegrityFailure`;
- `IntegrityResult`.

Los modelos públicos no contienen:
- ETag;
- filesystem path;
- Blob URL;
- container;
- Cosmos partition;
- Azure exception.

## Errores públicos

```text
SourceError
SourceReleaseNotFoundError
SourceConcurrencyError
SourceCorruptionError
SourceUnavailableError
SourceInvalidCursorError
```

Los providers deben traducir sus errores técnicos al vocabulario Source.

## Local provider

Estado: **IMPLEMENTED + VALIDATED**

Implementa:
- persistencia durable;
- releases inmutables;
- manifest único;
- concurrency token opaco;
- locking real entre procesos;
- CAS de promoción;
- History por predecessor chain;
- cursor opaco;
- integridad;
- restart/recovery;
- orphans no visibles.

## Azure Blob provider

Estado: **NEXT**

Debe implementar exactamente este contrato.

Blob puede utilizar ETag internamente como mecanismo de conditional write.

ETag no forma parte del contrato público.

## Storage connectivity

Atlanticus dispone de `connectivity/storage`.

Es conectividad técnica reutilizable.

El incremento Blob debe auditarla primero y modificarla sólo si se demuestra una capability técnica faltante real.

No duplicar en Source funciones genéricas que pertenezcan al contrato Storage.

## Fuera del core 1A.1

No forman parte de `SourceStore`:
- Projection;
- restore orchestration;
- Manager workspace;
- compare UI;
- cleanup/GC;
- retention policy;
- domain validation;
- users/authorization.
