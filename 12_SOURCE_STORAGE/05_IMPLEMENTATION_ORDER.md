# Source Storage — Implementation Order

Estado: **CURRENT PLAN**

## Checkpoint

```text
SOURCE-1A.1                       Core + Local                 CLOSED / VERIFIED
SOURCE-1A.2                       Blob                         CLOSED / VERIFIED
Projection                        Exact-release Core           CLOSED / VERIFIED
USERS-CANONICAL-PROJECTION-2      Users/Cosmos provider        CLOSED / VERIFIED
MANAGER-ROOT-CANONICAL-CUTOVER    Root Projection transport    CLOSED / VERIFIED
Users runtime exact provenance    Next isolated frontier       PLANNED
```

## Orden y estado

1. **CLOSED / VERIFIED** — Auditar Source existente por dominio.
   - Navigation Configuration File/SharePoint eran bindings Source legacy.
   - Cosmos Configuration es Projection.
   - No mover ownership Source a backend jobs.

2. **CLOSED / VERIFIED** — Auditar manifest existente.
   - `backend/configuration/manifest.py` no es el functional Source manifest.
   - Se congeló un manifest Source propio.

3. **CLOSED / VERIFIED** — Congelar Release Model.

4. **CLOSED / VERIFIED** — Congelar `SourceStore`.

5. **CLOSED / VERIFIED** — Congelar concurrencia/promoción de current.

6. **CLOSED / VERIFIED** — Implementar provider Local durable.

7. **CLOSED / VERIFIED** — Validar Core + Local.
   Gates cubiertos:
   - first publish durable/restartable;
   - same-content republish crea release distinta con mismo hash;
   - immutable history;
   - basis/provenance separado de predecessor;
   - stale snapshot conflict;
   - concurrent first publish con un único winner;
   - cross-process CAS;
   - orphans fuera de History;
   - pagination estable;
   - filtros temporales;
   - cursor validation;
   - missing resource;
   - digest/content-hash corruption;
   - read de release corrupta rechazado;
   - regresión Web completa;
   - Ruff/format verdes en integración real.

8. **CLOSED / VERIFIED — SOURCE-1A.2** — Implementar Blob como segundo provider.
   - `connectivity/storage` auditado.
   - `upload_if_match` agregado como única capability técnica faltante demostrada.
   - Mismo contrato público que Local.
   - ETag privado al provider; `ConcurrencyToken` público derivado del manifest.
   - first publish create-only y updates por If-Match.
   - immutable candidates + manifest único.
   - History por predecessor chain; orphans fuera de History.
   - integrity/restart/recovery validados.
   - tests deterministas Blob GREEN.
   - regresión Web GREEN.
   - 7 pruebas Azurite GREEN: restart/read/verify, stale token, first-publish race, update race con ETag real, orphan fuera de History, corrupción y ACK recovery.

9. **CLOSED / VERIFIED** — Añadir `source_release_id` a Projection y proyectar release exacta.
   - Projection Core implementado en `web/capabilities/projection/core`.
   - package `atlanticus-web-projection==0.1.0`.
   - target exacto `SourceKey + SourceReleaseRef`.
   - `project(target)` no consulta Source current.
   - provenance durable con `source_release_id`.
   - retry del mismo target sin republish.
   - alignment `NEVER_PROJECTED / CURRENT / OUTDATED`.
   - attempt outcome `SUCCESS / FAILED`.
   - 15 tests Projection GREEN.
   - regresión Web: 327 passed, 7 skipped.
   - Ruff/format Projection GREEN.

10. **CLOSED / VERIFIED — USERS-CANONICAL-PROJECTION-2** — Materializar Projection canónica Users/Cosmos con exact-release provenance, create-only + ETag/CAS e idempotencia same-target.

11. **CLOSED / VERIFIED — MANAGER-ROOT-CANONICAL-CUTOVER** — Reemplazar el contrato raíz de la acción Project por `ProjectionTarget` exacto.
   - `get_current_projection_target()` forma parte del workflow root.
   - `project(target)` transporta el target exacto.
   - callback selecciona current en servidor inmediatamente antes de ejecución.
   - browser no aporta la identidad ejecutable.
   - signal exact-release.
   - target histórico explícito preservado.
   - 76 tests Manager GREEN.
   - suite Web: 514 passed, 7 skipped.
   - Ruff/lock/diff check GREEN.

   Este step no representa una migración completa de todos los modelos administrativos basados en `source_revision: str`.

12. **PLANNED — siguiente frontera aislada recomendada** — Migrar provenance Managed de `users.runtime` a identidad exact-release.
   - no equiparar `UsersConfigurationBundle.revision` con `SourceReleaseId`;
   - no introducir shim release/string;
   - conservar ownership y CAS ya cerrados del writer Managed;
   - definir primero el contrato durable exacto antes de consumidores.

13. **PLANNED** — Migrar consumidores administrativos por dominio sobre los contratos canónicos reales de composición.

14. **PLANNED** — Retirar bindings Source legacy de SharePoint/Power Automate y contracts legacy sólo cuando no existan consumidores productivos.

## Regla de reemplazo

Cuando un incremento sustituya implementación existente, entregar explícitamente:

```text
DELETE
- ruta exacta

KEEP
- ruta exacta

MODIFY/REPLACE
- ruta exacta

GATES AFTER DELETE
- comandos exactos
```

No dejar legacy temporal por defecto.

No eliminar un archivo completo si contiene responsabilidades que deben conservarse.

## Regla de UI

No empezar por UI de historial.

Manager y consumidores deben usar contratos Source y Projection estabilizados.

## Regla de chat/checkpoint

Cuando un step queda CLOSED / VERIFIED:
1. actualizar canonical;
2. validar el diff;
3. cerrar el chat;
4. abrir un chat nuevo para el siguiente step.
