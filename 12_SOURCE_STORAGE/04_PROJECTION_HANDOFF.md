# Source Storage — Projection Handoff

Estado: **CLOSED / VERIFIED / CURRENT**

Projection trabaja sobre un release Source concreto.

No lee un `latest` mutable durante la ejecución de una proyección.

## Target exacto

El target ejecutable queda congelado como:

```text
SourceKey + SourceReleaseRef
```

`SourceReleaseRef` conserva la referencia resoluble exacta de la publicación.

`SourceReleaseId` sigue siendo la identidad de la publicación y no equivale a `content_hash`.

## Selección vs ejecución

Observar Source current y ejecutar Projection son operaciones distintas.

La selección de un target puede consultar:

```text
SourceStore.get_current(source_key)
```

La ejecución:

```text
project(target)
```

resuelve exclusivamente:

```text
SourceStore.read_release(
    target.source_key,
    target.source_release,
)
```

`project(target)` no vuelve a consultar Source current antes ni después de persistir la Projection.

## Source avanza durante Projection

Ejemplo:

```text
target seleccionado = V48
Source current       = V49
Projection ejecutada = V48
```

La ejecución de V48 sigue siendo válida.

Después de completarla:

```text
alignment = OUTDATED
```

No se convierte un éxito exact-release en fallo sólo porque Source haya avanzado.

## Provenance durable

La Projection activa identifica explícitamente:

- `source_key`;
- `source_release_id`;
- `source_published_at_utc`;
- `projected_at_utc`.

`source_release_id` identifica inequívocamente qué publicación Source representa la Projection.

`source_published_at_utc` permite reconstruir el `SourceReleaseRef` sin consultar un current mutable.

## Estado observable

Alignment durable:

```text
NEVER_PROJECTED
CURRENT
OUTDATED
```

Attempt outcome:

```text
SUCCESS
FAILED
```

Son dimensiones distintas.

Ejemplo:

```text
Projection activa = V47
Source current     = V48
Attempt Project(V48) = FAILED
```

Resultado:

```text
alignment = OUTDATED
attempt   = FAILED
```

`FAILED` no reemplaza ni borra el estado de la última Projection exitosa.

## Regla CURRENT / OUTDATED

La comparación se hace por identidad de publicación:

```text
projected.source_release_id == source.current.release_id
```

Nunca por `content_hash`.

Dos releases Source con el mismo contenido siguen siendo publicaciones distintas.

## Fallo y retry

Si Projection falla:

- Source release queda intacto;
- Source history no se reescribe;
- la última Projection exitosa permanece como referencia activa del dominio;
- el intento fallido conserva el target exacto;
- puede reintentarse ese mismo target;
- el retry no requiere republish de Source.

## Projection Store Core

El contrato Core expone:

```text
get_active(source_key)
replace_active(projection)
```

Core modela una Projection activa por `SourceKey`.

No se introduce en este hito:

- history genérico de Projection;
- failure journal durable genérico;
- generations/manifests de Projection;
- codec de dominio genérico.

La atomicidad/durabilidad concreta de `replace_active` debe ser satisfecha por cada provider concreto y validarse en su incremento correspondiente.

## Boundary

Projection Core:

- depende del contrato Source;
- no conoce paths físicos Source;
- no conoce Blob URLs;
- no conoce ETags;
- no determina Source current desde Cosmos;
- no pertenece a Navigation Configuration;
- no implementa lógica específica de un dominio consumidor.

El payload proyectado pertenece al dominio consumidor.

## Evidencia de cierre Core

Implementación:

```text
web/capabilities/projection/core
```

Package:

```text
atlanticus-web-projection==0.1.0
```

Baseline implementado:

```text
moragaga/atlanticus@5b383a3ff4dcbb2cc15f55df4819ebf9e61e63b4
```

Gates ejecutados en workspace real:

- 15 tests Projection GREEN;
- suite Web global: 327 passed, 7 skipped;
- Ruff Projection GREEN;
- Ruff format Projection GREEN.

## Evidencia domain/provider posterior — Users Cosmos

`USERS-CANONICAL-PROJECTION-2` cerró un provider concreto que satisface el contrato Core:

```text
ProjectionStore[UsersConfigurationCatalog]
→ CosmosUsersConfigurationProjectionStore
```

Checkpoint:

```text
moragaga/atlanticus@139ee93a118e51f66c3d585f00235f212a2475c1
```

Contrato verificado:
- first active write create-only;
- reemplazo mediante ETag/CAS;
- nunca blind upsert;
- same exact release + same payload = retry idempotente;
- same `SourceReleaseId` con metadata o payload incompatible = invariant failure;
- conflicto concurrente same-target converge;
- conflicto concurrente different-target falla explícitamente;
- no se infiere ordering por release ID ni `projected_at_utc`;
- target histórico exacto puede activarse explícitamente.

Esta evidencia es específica de Users/Cosmos y no congela la estrategia de providers de otros dominios.

## Fuera de este cierre

Permanecen abiertos:

- providers Projection Local/Cosmos concretos de otros dominios cuando sean necesarios;
- Manager productive BASE/SOURCE/WORKSPACE/PROJECTION cutover;
- migración de consumidores administrativos Navigation/Users y otros;
- Projection planner/orchestration multi-capability;
- derived resolutions;
- idempotencia provider/domain-level de otros providers;
- retention/GC operacional.

El cierre de Projection Handoff y del provider Users/Cosmos no implica cerrar esas capas.
