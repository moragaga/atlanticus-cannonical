# ADA Manager — Datos operacionales: catálogo y asignaciones

Estado: **CURRENT** para dominio, persistencia y UI anterior; **M01 CLOSED / CURRENT** como capacidad genérica; **M02 PLANNED / DESIGN** como adopción ADA. Corte focal: **2026-09-29**. No recalifica KPI, Alarm Engine ni otros frentes.

## Autoridad y evidencia

- `moragaga/atlanticus:main@9cc2cebe595ef1341830374ad2bb3c61baf6f5a2`: HEAD remoto comprobado; contiene **únicamente el incremento genérico M01** respecto de su parent `2e7500a6b8b4d5bbdad26d807abfa57936db99d5`.
- El dominio ADA inspeccionado está en `scopes/ada/web/operational-identification/` y el consumidor actual en `scopes/ada/web/application/ada-configuration-manager/src/ada/web/application/configuration_manager/`. M01 **no los modifica**; no llamar a M02 CURRENT.
- Usuario: 82 pruebas Manager PASS después de ordenar imports de producción y espejo; `git diff --check` PASS. Gates operacionales históricos de **25 PASS** y otro gate Manager anterior de **13 PASS** conservan su fecha y ámbito, sin confundirse con los 82 de M01.
- `atlanticus-decisions:main@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e` contiene reglas generales vigentes del Manager. Las decisiones históricas que no se hayan inspeccionado individualmente no pueden atribuirse a este corte.
- **UNVERIFIED**: qualification visual ADA con M01, integración M02, CI limpia/global, Entra/Azure real y warmup/snapshot futuros.

## Modelo de dominio ADA — CURRENT / FROZEN durante M02

```text
AREA        mina | planta
GROUP       1 | 2 | 3 | 4
POSITION    id estable generado por backend; label editable; active boolean
ASSIGNMENT  user_id + area_id? + position_id? + group_id?
```

Los campos de asignación son opcionales; `None` es legítimo. Área y grupo son referencias de dominio, no catálogos editables. Las etiquetas de cargo son únicas case-insensitive; la identidad del cargo es inmutable; se desactiva, no se elimina retrospectivamente. El backend admite asignaciones solo a usuarios promovidos. Cambiar a un cargo **nuevo o diferente** exige catálogo Source proyectado a la revisión actual y cargo activo; conservar uno ya asignado permite editar otros atributos aunque ese cargo esté inactivo o la proyección del catálogo esté pendiente. `operational.manage` protege administración y no concede automáticamente privilegios de sesión al usuario asignado.

Los atributos operacionales pertenecen a `ada.web.operational.identification`, **no** a `UserRecord`, `EffectiveUser`, Profiles, Access ni Atlanticus Users. Cambiar una etiqueta del catálogo no reescribe masivamente asignaciones individuales: estas conservan IDs.

## Persistencia — CURRENT / FROZEN

| Superficie | Identidad / contrato |
|---|---|
| Catálogo Source | `SourceKey('ada-operational-catalog')`; Source durable e historial propios. |
| Usuario Source | `SourceKey('ada-operational-user:' + user_id)` por usuario, CAS/revisión independiente. |
| Codec Source | Recurso `operational/data.json.gz`, `schema_version=1`, tipo `catalog`/`assignment`. |
| Catálogo proyectado | `ada_operational_catalog_projection`, revisión exacta de Source, posiciones/áreas/grupos. |
| Usuario proyectado | `ada_operational_assignment_projection`, IDs y `source_release_id`, particionado por SourceKey. |
| Persistencia Cosmos | `CosmosOperationalProjectionStore`, CAS ETag con reintentos acotados; contenedor inyectable. |

Source y Projection son dos pasos, **sin transacción distribuida**. Una publicación exitosa y proyección fallida necesita reintento desde Source durable. Runtime y workers consumen proyecciones Cosmos, no Blob. La asignación individual no debe bloquearse ni reescribirse por el lifecycle del catálogo salvo las validaciones de referencia ya existentes.

## Implementación administrativa observada en main — CURRENT ANTERIOR A M02

- `operational.py` registra **`ManagerEntry`** `key='operational-identification'`, grupo `administration`, título «Datos operacionales», ruta `/operational-identification` y permiso `operational.manage`.
- `operational_layout.py` implementa su **propio selector** «Asignación» / «Datos operacionales»; incluye búsqueda, paginación, modal por usuario y modal de cargo, estado/trazabilidad y reintento de proyección.
- `operational_callbacks.py` publica el catálogo directamente al guardar un cargo (`publish_catalog`/`create_position` y después `project_current`); las asignaciones individuales usan `publish_assignment` + `project_current` inmediatamente.
- `operational_catalog_workflows.py` **ya define** `OperationalCatalogDraftEditor`, `OperationalCatalogManagerSourceWorkflow`, `OperationalCatalogManagerDraftValidationWorkflow` y `OperationalCatalogManagerContracts`. `wiring.py` compone los contratos, y `composition.py` registra los servicios cuando están disponibles. Su existencia **no** significa que la UI operacional actual use el workflow Manager.
- Los nombres de proveedor de la entrada operacional se obtienen actualmente de `tools_source_name`/`tools_projection_name` en `composition.py`; confirmar la semántica requerida antes de un cambio, no reutilizar etiquetas incorrectas por comodidad.

## M01 disponible; adaptación ADA M02 aún PLANNED

`ManagerCompanionView(title, layout)` se incorpora opcionalmente a `ManagerModule` mediante `companion_view`, `primary_view_title` y `default_primary_view`. La vista administrativa conserva configuración/workspace y workflow; la companion no participa en ese lifecycle. El selector no publica, proyecta ni muta el workspace.

**PROPOSED para M02 / acuerdo conversacional posterior a la redacción del canonical previo**, sujeto a conciliación formal:

```text
Manager / Datos operacionales
├── Asignaciones           companion, inicial, escritura individual inmediata
└── Catálogo de cargos     módulo administrativo
    ├── Configuración      edición de borrador, crear/editar/desactivar
    └── Estado y trazabilidad
        validate -> verify Source -> publish -> project -> history
```

La pantalla M02 no conserva un segundo sistema de pestañas duplicado dentro de ADA. La decisión anterior documentada en este archivo era «Datos operacionales» primero y «Asignación» segundo. **CONFLICT DOCUMENTAL / OPEN:** el diseño posterior del chat propone «Asignaciones» primero y «Catálogo de cargos» segundo; confirmar que este delta reemplaza formalmente el orden y los labels previos **antes de implementarlo**. El código `main` todavía refleja su UI anterior: esa diferencia con el objetivo futuro es un **GAP** esperado, no una implementación M02.

Los modales seguirán la convención de Perfiles. Las listas de ambas vistas deben reservar altura proporcional a la página seleccionada de **10/20 filas** aun con pocos resultados o página final, sin filas ficticias; validar apariencia manualmente y automatizar comportamiento, no CSS visual.

## Otras fronteras abiertas, independientes de M02

- **Snapshot consolidado: PLANNED / TECH CONTRACT OPEN.** Un solo archivo sobrescrito sin versionado propio con usuarios que tienen al menos uno de los tres atributos no nulo, exclusivamente para recuperación conjunta. No está implementado. Faltan esquema/ruta, inventario de Sources/usuarios retirados, concurrencia, atomicidad y recovery. No reemplazar ni borrar los Sources individuales sin decisión explícita de migración.
- **Sesión: PLANNED.** Tras identidad/promoción Entra, consulta individual en Cosmos y resolución de etiquetas desde catálogo. Usuario no promovido `guest`; tras promoción se requiere recarga. Wiring productivo no verificado.
- **Warmup: PLANNED.** Solo Profiles y catálogo operacional por proceso; no usuarios, promociones, asignaciones ni snapshot. Refresco 10 minutos = PROPOSED, no scheduler implementado.
- **Ruff ajeno a M01:** seis avisos baseline Manager; `I001` histórico en `operational-identification/service.py` sigue **UNVERIFIED en HEAD actual** sin revalidación focal, no asumir que continúa ni que quedó corregido.

El siguiente chat debe contrastar este documento contra implementación y decisiones actuales y no iniciar cambios M02 antes de resolver su conflicto de orden/labels y el circuito de workspace/catalog UI.
