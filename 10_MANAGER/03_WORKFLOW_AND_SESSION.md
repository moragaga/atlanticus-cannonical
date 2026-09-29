# Manager — Workflow and Session

Estado: **CURRENT**. Actualización focal M01: **CLOSED / IMPLEMENTED + VALIDATED localmente** a 2026-09-29. El resto de los checkpoints históricos de este documento no se recalifica por M01.

## Autoridad del delta M01

- Implementación: `moragaga/atlanticus:main@9cc2cebe595ef1341830374ad2bb3c61baf6f5a2`. HEAD remoto y contenido de los 12 archivos del commit comprobados el 2026-09-29; parent `2e7500a6b8b4d5bbdad26d807abfa57936db99d5`.
- Intención general: `moragaga/atlanticus-decisions:main@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`, especialmente `manager_decisions/ATLANTICUS_MANAGER_GLOBAL_RULES_2026-09-02.md`.
- Qualification M01: salida de terminal aportada por el usuario, **82/82 pruebas** del Manager aprobadas tras corregir producción y espejos, `git diff --check` sin incidencias. No equivale a CI, checkout limpio ni E2E visual de ADA.

## Flujo conceptual de ManagerModule — CURRENT / FROZEN

```text
WORKSPACE -> validate -> verify Source -> publish Source -> project -> history/preview
```

Guardar WORKSPACE **no** publica Source. El flujo corresponde a `ManagerModule` y no se aplica automáticamente a `ManagerEntry` ni a la vista complementaria.

```text
BASE        SourceSnapshot observado al establecer o rebasar el workspace
SOURCE      current durable autoritativo, independiente del workspace
WORKSPACE   payload editable local + revisión local + BASE
PROJECTION  proyección activa de una Source release exacta
```

`ManagerModule` conserva `source_key`, `source_service`, `source_reader_service`, `projection_service`, `draft_validation_service`, `source_history_service | None`, `access_key | None` y la composición de su WebModule. `ManagerEntry` conserva `key`, `group_key`, `title`, `route`, `order`, `layout`, `description`, `access_key | None` y `web_module | None` sin simular servicios de Source/Projection.

## M01 — vista complementaria opcional, CURRENT

La nueva extensión genérica pertenece a `web/capabilities/manager` y **no conoce ADA**:

```python
ManagerCompanionView(title: str, layout: ManagerLayoutFactory)
ManagerModule.companion_view: ManagerCompanionView | None = None
ManagerModule.primary_view_title: str | None = None
ManagerModule.default_primary_view: str = 'module'
```

- `default_primary_view` acepta exactamente `'module'` o `'companion'`. Elegir `'companion'` exige `companion_view` configurada; los títulos no pueden estar vacíos.
- Un módulo **sin** companion conserva el layout anterior; no se introducen pestañas principales innecesarias.
- Con companion se muestran las dos vistas principales: la complementaria y la administrativa del módulo. Cada una tiene sus IDs aislados por módulo. La configuración decide cuál se selecciona inicialmente.
- La vista administrativa (`module`) conserva **Configuración / Estado y trazabilidad**, su WORKSPACE y el lifecycle Source/Projection. La complementaria se monta como contenido separado y **no recibe** el workflow del módulo.
- Cambiar de vista solo cambia presentación/visibilidad. No publica, proyecta, modifica el workspace ni desmonta el contenido administrativo para reconstruirlo en cada click.
- M01 no define semántica de escritura de una vista complementaria: **cada consumidor de dominio** es responsable de ella. No inventar otro Source genérico ni trasplantar la publicación del módulo a la companion.

**Pruebas locales M01 comunicadas:** cuatro casos específicos de `test_companion_view.py` cubren selección por defecto, ausencia de companion, validaciones de configuración y cambio de visibilidad; 82 pruebas totales Manager aprobadas, incluidos los contratos de espejo AST. Seis incidencias Ruff preexistentes permanecen separadas: `source.py` I001; `layout.py` F401 `ManagerError`; `test_brand_header.py` F401; `test_registry.py`, `test_surface.py` y `test_workspace_contract.py` I001. No declarar Ruff global PASS ni reabrir M01 para resolverlas.

## Autorización y ownership — CURRENT / FROZEN

`ManagerAuthorizationPolicy.can_view(principal, item)` se aplica a `ManagerModule | ManagerEntry`; el coordinator vuelve a comprobar capacidad para operar. `is_local` o un perfil nominal de administrador no otorgan permisos implícitos. No crear gates genéricos paralelos `can_validate`, `can_publish` o `can_project` sin decisión nueva.

Home, sidebar y routing derivan del `ManagerModuleRegistry`; Home `/manager` no redirige al primer módulo. La Home conserva seis cards por página y el sidebar filtra elementos **después** de resolver visibilidad. Las cards nunca ejecutan workflow. La separación entre contenido administrativo y lifecycle sigue vigente.

## Source, Projection y Workspace — CURRENT / FROZEN

- `SourceReaderWorkflow`, `SourcePublicationWorkflow`, `SourceHistoryWorkflow` conservan identidad de release y `SourceSnapshot`; un historial lee la release solicitada, no la actual por conveniencia.
- Manager transporta un `ProjectionTarget` completo: `SourceKey`, release exacta y dependencias. No reconstruirlo desde cadenas de revisión ni alterar el target en reintentos.
- `ManagerWorkspace` separa identidad local del payload de Source durable. ADA Configuration Manager utiliza `ManagerWorkspaceBridge` donde ya está compuesto. Profiles conserva su composición reusable y Users su lifecycle propio como `ManagerEntry`.
- En cambios de ruta, el callback de workflow con outputs montados pero sin módulo visible usa `PreventUpdate`; las acciones de Source/Projection ignoran `ManagerEntry`.

## Implementaciones históricas no reabiertas por M01

- Profiles y Users tienen sus propias composiciones; no duplicar sus servicios en ADA.
- El contrato administrativo Users actual gestiona promoción y edición mediante `UsersAdministrationService`, con identidad del directorio read-only y selección explícita de snapshots Profiles.
- Los antiguos `ManagerModuleAccess`, `ConfigurationLifecycleWorkflow`, interfaces `Exact*Workflow`, `workflow_service`, `exact_source_*`, `exact_projection_service`, `expected_source_revision` y reconstrucción de ProjectionTarget a partir de revisión fueron eliminados: **SUPERSEDED**; no introducir aliases, shims ni legacy.
- Los hallazgos históricos sobre un consumer Navigation authorization desalineado y la qualification de otros frentes requieren revalidación en su propio alcance; no convertirlos en resultados de M01.

## Frontera siguiente de este chat — PLANNED

Integrar exclusivamente la capacidad ya publicada de M01 en **ADA Datos operacionales (M02)**, después de reconciliar el orden/nombres de vistas frente a `10_ADA_OPERATIONAL_IDENTIFICATION_BOUNDARY.md` y al código actual. M02 no está implementado y **no** modifica contratos de Source, Projection, Users, Access ni Profiles. Ver `11_ADA_OPERATIONAL_DATA_ROADMAP.md`.
