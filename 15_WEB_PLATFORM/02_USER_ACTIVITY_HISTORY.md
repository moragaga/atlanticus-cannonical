# Web Platform — User Activity by Page

Estado: **CURRENT / BASELINE 1.0**

## Objetivo

No se intenta reconstruir el recorrido exacto click por click.

Para User Activity importa:

- qué página usó el usuario;
- cuántas veces accedió;
- cuánto tiempo activo acumuló;
- primera actividad;
- última actividad;
- aplicación;
- sesión;
- viewport/dispositivo útil.

La trazabilidad secuencial fina se reserva para dominios donde el orden es parte del negocio, como Alarm Journey.

## Unidad persistida

```text
1 documento
=
1 application
+ 1 user/actor
+ 1 client session
+ 1 page/route
```

Si el usuario vuelve a la misma página dentro de la misma sesión:

```text
mismo documento
→ visit_count += 1
→ active_seconds += nueva actividad
→ last_seen_at = ahora
```

Si cambia de página:

```text
otro page contract/document
```

## Session

La sesión **no se pierde por un full browser reload**.

El `client_session_id` debe sobrevivir el reload de página mientras siga siendo la misma sesión funcional.

Cambiar de página conserva la misma sesión, pero materializa otro documento por route/page.

## Watcher

Se preserva el watcher actual de cinco minutos.

La actividad se pausa cuando:

- la página pierde foco/visibilidad según contrato existente;
- la sesión deja de estar activa;
- ocurre pagehide/unload según implementación.

Al volver, continúa la misma sesión si el session contract sigue vigente.

El watcher no crea un documento nuevo por sí mismo.

## Identidad del documento

`id` determinístico para:

```text
application
+ actor
+ client_session_id
+ route/page
```

El formato exacto puede ser normalizado o hash.

## Partition key

**OPEN / NO FREEZE AÚN.**

Candidato inicial:

```text
actor_key / user_id
```

pero no se congela hasta revisar:

- volumen;
- consultas;
- cardinalidad;
- distribución;
- multi-application use.

El `id` garantiza identidad del documento; partition key resuelve distribución/consulta y no necesita ser única por fila.

## Modelo candidato

```text
UserPageActivity
├── id
├── partition_key
├── application_key
├── actor_key
├── client_session_id
├── route_key
├── pathname
├── first_seen_at_utc
├── last_seen_at_utc
├── active_seconds
├── visit_count
├── initial_viewport
├── last_viewport
├── initial_screen
├── last_screen
├── last_sequence
└── last_event_id
```

## TTL

```text
86400 segundos = 24 horas
```

No usar este container como warehouse histórico.

## Session Summary

No se persiste otro documento summary.

El dashboard calcula:

```text
active_seconds = sum(page.active_seconds)
page_views = sum(page.visit_count)
pages = count(page documents)
first_seen = min(page.first_seen)
last_seen = max(page.last_seen)
```

## Navigation

Navigation puede enriquecer `route_key` si está instalada.

No es una dependencia obligatoria.
