# Source Storage — Implementation Order

Estado: **CURRENT PLAN**

## Checkpoint

```text
SOURCE-1A.1  Core + Local  CLOSED / VERIFIED
SOURCE-1A.2  Blob          NEXT
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

8. **NEXT — SOURCE-1A.2** — Implementar Blob como segundo provider.
   - Auditar primero `connectivity/storage`.
   - No modificar Connectivity salvo capability técnica faltante demostrada.
   - Mantener el mismo contrato público.

9. **PLANNED** — Añadir `source_release_id` a Projection y proyectar release exacta.

10. **PLANNED** — Integrar Manager BASE/SOURCE/WORKSPACE/PROJECTION, history/compare/conflict.

11. **PLANNED** — Migrar dominios/consumidores progresivamente.

12. **BLOCKED UNTIL PARITY** — Retirar bindings Source legacy de SharePoint/Power Automate sólo con Blob parity y recovery validados.

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

Manager debe consumir contratos Source y Projection ya estabilizados.

## Regla de chat/checkpoint

Cuando un step queda CLOSED / VERIFIED:
1. actualizar canonical;
2. validar el diff;
3. cerrar el chat;
4. abrir un chat nuevo para el siguiente step.
