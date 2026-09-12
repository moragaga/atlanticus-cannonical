# Manager — Workflow and Session

Estado: **CANDIDATE / STORAGE BINDING EN MIGRACIÓN**

## Flujo conceptual

Manager conserva separación entre configuración editable y workflow administrativo.

Estados/acciones actuales incluyen conceptualmente:

- working draft;
- saved local/browser draft;
- validate;
- verify source;
- publish/save to Source;
- project;
- history/preview;
- conflict handling.

## Session

Decisiones históricas congelaron:

- primera visita hidrata desde Source;
- navegación interna reutiliza snapshot/working state;
- paginación/filtro local no relee Source;
- verificaciones autoritativas se repiten cuando una operación destructiva o de publicación lo exige;
- no hidratar preventivamente todo el Manager sólo para pintar la pantalla.

Estas invariantes sobreviven al cambio SharePoint → Blob.

## Integridad

La UI anticipa problemas pero no es autoridad final.

Ejemplos:
- referencias antes de delete;
- source version conflict;
- validación antes de publicación.

Backend conserva la última garantía autoritativa.

## Draft

Guardar draft no equivale a publicar Source.

Con Blob Source:
- draft no genera `source_release`;
- sólo una publicación efectiva genera una nueva versión lógica.

## Concurrencia

Frontend:
- detecta;
- explica;
- permite inspeccionar;
- deja decisión humana.

Backend:
- aplica precondición autoritativa de promoción/escritura.

No implementar merge automático sin contrato de dominio explícito.

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
