# Manager — Workflow and Session

Estado: **CURRENT CONTRACT / ROOT PROJECTION CUTOVER CLOSED**

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

`web/capabilities/manager/.../workspace.py` implementa los contratos base:
- `ManagerWorkspace`;
- `ManagerSourceVerification`;
- `ManagerPublicationContext`;
- `SourceSnapshot`;
- `ConcurrencyToken`;
- `SourceReleaseRef`;
- selección de `ProjectionTarget`.

Estado:

```text
contracts/model                         CURRENT
root Projection action cutover          CLOSED / VERIFIED / CURRENT
administrative publish/workspace legacy CURRENT
browser persistence                     PLANNED
```

La formulación anterior “productive coordinator cutover” queda refinada: el hito cerrado reemplaza el contrato raíz de la acción Projection, no todas las revisiones textuales usadas todavía por publicación/verificación/history administrativa.

No crear un segundo coordinator paralelo ni un shim `SourceReleaseId <-> str` para completar consumidores posteriores.

## Root Projection action

Contrato congelado:

```text
ConfigurationLifecycleWorkflow
    get_current_projection_target() -> ProjectionTarget | None
    project(target: ProjectionTarget) -> ProjectionExecutionResult

ProjectionExecutionResult
    target: ProjectionTarget
```

`ManagerProjectionCoordinator.project(...)` recibe un `ProjectionTarget` y lo entrega al workflow sin convertirlo a string, normalizarlo ni volver a seleccionar Source current.

La UI productiva de Project:
1. obtiene el principal y módulo en servidor;
2. selecciona `get_current_projection_target(...)` en servidor;
3. ejecuta inmediatamente `project(..., target)`;
4. no confía en un target/revision serializado desde browser state;
5. emite provenance observable de la release exacta.

Signal de Projection:

```text
source_key
source_release_id
source_published_at_utc
projection_revision
```

No contiene `source_revision` como identidad de ejecución.

El botón Project depende de que exista un target canónico exacto, no de la comparación legacy `active_source_revision != source_revision`.

## Exact target y concurrencia de selección

Selección y ejecución son pasos distintos.

Una vez seleccionado el target:

```text
project(target)
```

no vuelve a consultar Source current.

Por tanto es válido:
- seleccionar V48;
- que Source avance a V49;
- ejecutar V48;
- terminar con éxito;
- observar después `OUTDATED`.

El coordinator también acepta un target histórico explícito y lo transporta intacto; no lo reemplaza por current.

## Qué permanece legacy

El cierre root Projection no elimina todavía todos los campos textuales administrativos.

Permanecen fuera de este hito, entre otros:
- `ProjectionStatus.source_revision` y `active_source_revision` como estado administrativo legacy;
- `SourcePublicationResult.source_revision`;
- `SourceVerificationResult.source_revision`;
- `publish_draft(... expected_source_revision: str | None)`;
- history/load revision textual;
- workspace/browser persistence aún no migrada a IndexedDB.

Esos contratos no deben reinterpretarse como `SourceReleaseId` ni convertirse mediante shim temporal.

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

La implementación IndexedDB pertenece a un incremento posterior y no al root Projection cutover.

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

Manager selecciona un target exacto:

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

Este root handoff quedó implementado y validado en:

```text
moragaga/atlanticus@5fd2858c4bd19c8f9cc416e0996162cb7a3f8c06
```

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

Estado: **PLANNED**. No fue parte de `MANAGER-ROOT-CANONICAL-CUTOVER`.
