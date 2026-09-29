# ADA Datos operacionales — roadmap y fronteras abiertas

Estado: **CURRENT PLAN / corte 2026-09-29**. Este documento ordena el **frente específico** de Datos operacionales en el contexto del traspaso M01 → M02. No reemplaza la prioridad global del frente KPI Collector, Alarm Engine ni el trabajo separado de snapshot/sesión/warmup.

## Referencias verificadas

```text
atlanticus:main                 9cc2cebe595ef1341830374ad2bb3c61baf6f5a2
M01 parent                      2e7500a6b8b4d5bbdad26d807abfa57936db99d5
atlanticus-decisions:main       50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
canonical leído para el cierre  2e8bbf4780cafc4cea3b18351861aa97a4fb0053
```

El remote `atlanticus:main` fue leído después del último mensaje del usuario: el commit M01 **sí está publicado**, aunque antes de esa verificación el push se había considerado pendiente. `9cc2cebe` modifica exactamente once archivos de Manager y agrega `test_companion_view.py`; no modifica código ADA.

**Evidencia local del usuario:** 4 pruebas específicas M01 PASS; 82 pruebas totales de Manager PASS tras ordenar imports en producción y espejo; `git diff --check` PASS; Ruff reporta seis incidencias **anteriores a M01**, no cero. **UNVERIFIED:** CI, rerun limpio tras push, integración visual M01 sobre ADA, cualificación productiva.

## Estado por incremento

| Incremento | Estado | Alcance probado o frontera |
|---|---|---|
| Dominio operacional, Sources y proyecciones existentes | CURRENT / antecedentes VERIFIED | Código inspeccionado; 25 pruebas operacionales reportadas en otro hito; no rerun durante M01. |
| M01 Manager companion genérico | **CLOSED / CURRENT / IMPLEMENTED + VALIDATED LOCAL** | Commit remoto confirmado, API, layout y callbacks; 82 pruebas Manager locales. |
| M02 ADA Datos operacionales | **PLANNED / DESIGN; BLOCKED para codificación** | Requiere acuerdo explícito para reconciliar tabs y adaptar la UI al contrato M01. Ningún archivo ADA se ha implementado en M02. |
| Snapshot consolidado operacional | PLANNED / contrato funcional decidido; diseño técnico OPEN | Independiente de M02; implementación BLOCKED por inventario, schema, concurrencia, recuperación y migración. |
| Resolución operacional al iniciar sesión | PLANNED / UNVERIFIED wiring | Otro foco; utiliza proyección individual Cosmos. |
| Warmup catálogo Profiles + catálogo operacional | PLANNED | Otro foco; excluye usuarios/asignaciones. |
| Python 3.14.7 metadata | PLANNED / SEPARATE | `uv run` del workspace Manager selecciona 3.14.2; objetivo Project 3.14.7. |
| Seis hallazgos Ruff Manager baseline | OPEN / SEPARATE | No son regresiones M01. |

## M02: única frontera del chat siguiente

### Diseño a conciliar antes del primer cambio

El canonical anterior proponía **«Datos operacionales» primero y «Asignación» después**. El diseño posterior de este hilo propuso **«Asignaciones» (companion inicial) y «Catálogo de cargos» (módulo con workflow)**. No reinterpretar ni sustituir silenciosamente el contrato antiguo: registrar la aceptación del nuevo orden/labels o mantener el anterior si esa es la decisión explícita.

La conversión candidata conserva `key='operational-identification'`, ruta `/operational-identification`, grupo `administration`, permiso `operational.manage` y ownership de ADA. No crear otra ruta ni un módulo paralelo sin requisito verificado. M01 existe en `ManagerModule`, pero la UI actual sigue siendo `ManagerEntry`, con tabs y publicaciones locales; su sustitución limpia requiere revisar **antes** `operational.py`, `operational_layout.py`, `operational_callbacks.py`, `operational_ids.py`, `operational_catalog_workflows.py`, `composition.py`, `wiring.py`, `dependencies.py`, CSS operacional y pruebas existentes, más sus espejos pedagógicos.

### Reutilización existente; cero duplicación

- `OperationalCatalogDraftEditor` ya construye payloads de borrador y mantiene identidad de cargo; `OperationalCatalogManagerSourceWorkflow` y `OperationalCatalogManagerDraftValidationWorkflow` ya formalizan Source/validación; `OperationalCatalogManagerContracts` ya está compuesto condicionalmente y registrado en `composition.py`.
- **PROPOSED:** usar estos contratos en un `ManagerModule` real con `ManagerWorkspaceBridge` si es el puente apropiado después de inspeccionar la composición concreta. El código del host debe exponer claramente el workspace y las dependencias requeridas; **no inventar** nuevas clases o servicios si los anteriores satisfacen el flujo.
- En la companion, conservar `publish_assignment` y `project_current(assignment_source_key(user_id))` con concurrencia optimista, permisos y validación de cargo proyectado; no pasar asignaciones al workflow del catálogo.
- Quitar limpiamente el selector ADA anterior y la publicación inmediata del catálogo cuando el nuevo flujo esté operativo; sin legacy, adapters temporales ni rutas duplicadas.

### Criterios de aceptación propuestos para M02

1. Sin companion configurada, otros `ManagerModule` conservan comportamiento M01 existente. El nuevo módulo ADA presenta exactamente dos vistas principales, sin navegación duplicada; la predeterminada y sus nombres se validan contra la decisión reconciliada.
2. Crear/editar/desactivar cargo solo altera borrador antes de `validate`/`verify Source`/`publish`/`project` del Manager, con historial, conflicto y reintento separados. No mantener la escritura directa anterior.
3. Asignación individual sigue siendo inmediata y por usuario, con `SourceSnapshot` esperado, proyección y retry; editar etiquetas/catálogo nunca actualiza automáticamente todas las asignaciones.
4. Se preserva protección al seleccionar cargo activo/proyectado nuevo o diferente y se permite conservar el cargo ya asignado cuando corresponda.
5. Listas 10/20 con filtrado, page reset/pagination correctos; área/grupos solo informativos; modal como Perfiles; el alto reservado y responsive se comprueban **visualmente**, no con tests de CSS o snapshots de DOM de presentación.
6. Producción y espejo comentado tienen AST equivalente; pruebas de comportamiento focales y regresión ADA + Manager; ninguna edición fuera del scope acordado.

La validación de M02 está **UNVERIFIED** hasta ejecutar sus pruebas y revisión visual. No afirmar que M01 integra ya ADA.

## Frentes paralelos expresamente fuera de M02

**Snapshot único en Storage — significado funcional DECIDED:** un archivo sobrescrito, sin versionado propio, solo usuarios que tengan al menos uno de `area_id`, `position_id`, `group_id` informado, para recuperación conjunta y nunca para lectura operacional. El código actual tiene Source separados (catálogo + uno por usuario). El diseño técnico OPEN exige inventario de usuarios incluso retirados, ruta/schema, actualización atómica, CAS/concurrencia, recuperación, disparador y política de convivencia o migración. M02 **no queda bloqueado por el snapshot**, salvo que una dependencia real aparezca en revisión y sea explícitamente decidida; el trabajo de snapshot sigue BLOCKED por su contrato técnico.

**Sesión y warmup — separados:** la sesión Entra/promoción se resuelve por usuario, consulta proyección individual de Cosmos y enlaza catálogo operacional; `guest` para no promovidos y recarga tras promoción. Warmup de **solo Profiles y catálogo operacional**, sin listas de usuarios, promociones, asignaciones ni snapshot. El valor de 10 minutos es PROPOSED. No introducir Redis, scheduler, precargas ni alteraciones de otros frentes en M02.

**Higiene / metadata:** revalidar aisladamente el `I001` histórico del backend operacional antes de presentarlo como deuda CURRENT. Mantener registradas las seis incidencias Ruff Manager baseline. Objetivo Python 3.14.7, entorno Manager `==3.14.2`: cambio separado con lockfiles, nunca incidental.
