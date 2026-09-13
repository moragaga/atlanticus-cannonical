# Manager — Workflow and Session

Estado: **CURRENT CONTRACT / PRODUCT CUTOVER IN PROGRESS**

## Flujo conceptual

Manager conserva separación entre configuración editable y workflow administrativo.

Estados/acciones conceptuales:
- working workspace;
- persisted browser workspace;
- validate;
- verify source;
- publish/save to Source;
- project;
- history/preview;
- conflict handling.

## BASE / SOURCE / WORKSPACE / PROJECTION

El modelo canónico distingue:

```text
BASE
    snapshot Source exacto observado al establecer el workspace

SOURCE
    current durable autoritativo, independiente del workspace

WORKSPACE
    estado editable local + revision propia

PROJECTION
    active projection de una Source release exacta
```

`web/capabilities/manager/.../workspace.py` ya implementa los contratos base:
- `ManagerWorkspace`;
- `ManagerSourceVerification`;
- `ManagerPublicationContext`;
- `SourceSnapshot`;
- `ConcurrencyToken`;
- `SourceReleaseRef`;
- selección de `ProjectionTarget`.

Estado:

```text
contracts/model          CURRENT
productive coordinator  BLOCKED / IN PROGRESS
browser persistence     PLANNED
```

El coordinator productivo todavía usa contratos legacy basados en `source_revision: str`.
No crear un segundo coordinator paralelo ni un shim de compatibilidad como solución transitoria.

## Session

Invariantes:
- primera visita hidrata desde Source cuando no existe un workspace recuperable;
- navegación interna reutiliza working state;
- paginación/filtro local no relee Source;
- no hidratar preventivamente todo el Manager sólo para pintar la pantalla;
- una BASE stale nunca autoriza descartar o sobrescribir automáticamente el WORKSPACE;
- SOURCE se consulta de forma independiente para verificar conflicto antes de publicar.

## Browser WORKSPACE

Dirección decidida:

```text
dcc.Store(storage_type="memory")
    = estado activo de la sesión Dash

IndexedDB
    = persistencia browser del WORKSPACE

SourceStore / Blob
    = autoridad durable publicada

ProjectionStore
    = proyección activa durable
```

IndexedDB:
- no es autoridad;
- no sustituye Source;
- no debe almacenar secretos;
- perderlo sólo puede perder trabajo no publicado;
- no requiere gzip/base64 inicialmente;
- debe guardar el workspace estructurado;
- debe integrarse con Dash mediante JavaScript dedicado + `clientside_callback`.

`localStorage` queda reservado para estado/preferencias UI pequeñas; no es el almacenamiento del WORKSPACE.

La implementación IndexedDB pertenece al cutover posterior de Manager y no a los incrementos Source/Projection de dominio.

## Integridad

La UI anticipa problemas pero no es autoridad final.

Ejemplos:
- referencias antes de delete;
- Source conflict;
- validación antes de publicación.

Backend conserva la última garantía autoritativa.

## Draft / Workspace

Guardar WORKSPACE no equivale a publicar Source.

- WORKSPACE browser no genera `source_release`;
- sólo `SourceStore.publish(...)` genera una publicación Source;
- History durable contiene publicaciones Source reales, no autosaves del browser.

## Concurrencia

Frontend:
- detecta;
- explica;
- permite inspeccionar;
- deja decisión humana.

Backend:
- aplica la precondición autoritativa mediante `ConcurrencyToken`.

Publicación normal:
- BASE conserva la release sobre la que se inició el trabajo;
- SOURCE se verifica;
- la publicación usa el token autoritativo correspondiente.

Conflicto / overwrite autorizado:
- se conserva la BASE original como `basis_release`;
- se relee SOURCE current;
- se usa un `ConcurrencyToken` fresco;
- no existe bypass `force=True`.

No implementar merge automático sin contrato de dominio explícito.

## Source -> Projection

Manager debe seleccionar un target exacto:

```text
ProjectionTarget =
    SourceKey
    +
    SourceReleaseRef
```

La ejecución de Projection:
- no depende de un `latest` mutable;
- puede proyectar una release seleccionada aunque Source haya avanzado;
- puede terminar correctamente y quedar `OUTDATED`;
- un fallo no revierte Source ni reemplaza el último active projection exitoso.

El coordinator productivo todavía no completa este cutover.

## Multi-module projection bootstrap

El workflow normal por módulo permanece.

Adicionalmente, primera instalación requiere un coordinador de múltiples proyecciones.

Ese coordinador:
- toma únicamente módulos instalados;
- ordena por dependencias declaradas;
- no fuerza Users/Navigation/Tools/KPI cuando no existen;
- muestra estados y bloqueos;
- ejecuta idempotentemente;
- preserva historial/source authority.

La lógica de orden no pertenece a la UI del Manager.
