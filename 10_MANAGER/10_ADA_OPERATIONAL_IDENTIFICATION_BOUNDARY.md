# ADA Manager — Datos operacionales: catálogo y asignaciones

Estado: **CURRENT / BACKEND Y MANAGER IMPLEMENTADOS EN SU ALCANCE ACTUAL; UX REORDENADA Y SNAPSHOT CONSOLIDADO PLANNED**. Corte documental: **2026-09-29**. Este documento sustituye exclusivamente la clasificación histórica «PLANNED / NO IMPLEMENTATION» de esta capacidad, no reabre otros dominios del Manager.

## Autoridad y alcance de la constatación

- **VERIFIED — código remoto:** `moragaga/atlanticus:main@caced5d7711cf059d36ec61aecc9b3e9629bd41f`, leído el 2026-09-29. Inspección específica de `scopes/ada/web/operational-identification`, Manager, Source/Projection, Users y Storage. La rama canonical de partida para integrar este candidato era `moragaga/atlanticus-cannonical:main@ec16bd2ccf0ae06065b8ee1d3a231ef4d2cbac57`.
- **VERIFIED — ejecución local comunicada por el usuario:** `uv run --locked pytest` en `scopes/ada/web/operational-identification`: **25 passed en 0.13 s**. La ejecución separada del Manager previamente comunicada arrojó **13 passed**; corresponde a otro gate y no demuestra la suite global. El parche del gate de cargo proyectado fue aplicado y quedó incorporado a `atlanticus:main`, confirmado por inspección del código y sus pruebas.
- **VERIFIED — lint parcial pendiente:** `uv run --locked ruff check src tests` reportó únicamente `I001` en `src/ada/web/operational/identification/service.py` (bloque de imports). **No declarar Ruff PASS** ni asignar estado CLOSED a este detalle antes de corregir producción y espejo comentado y reejecutar el gate.
- **UNVERIFIED:** CI, tests monorepo, E2E Azure/Entra, ejecución productiva Blob/Cosmos, comportamiento visual final tras reorganización y warmup operacional. El objetivo general del Project es Python 3.14.7; `operational-identification/pyproject.toml` consultado aún exige `==3.14.2`. No afirmar alineación de metadata por la versión mostrada en el shell.
- `atlanticus-decisions` no se utiliza como prueba de implementación de este corte; la conciliación histórica integral de otros dominios es otro trabajo. Si una decisión vigente contradice el código, registrar conflicto, no reinterpretarla silenciosamente.

## Ownership y frontera CURRENT

- El dominio y sus contratos son propios de **ADA**, bajo `ada.web.operational.identification`. No extender `UserRecord`, `EffectiveUser`, Profiles, Access o Atlanticus Users con atributos específicos de ADA.
- El Manager consume `OperationalIdentificationService` como `ManagerEntry`, ruta `/operational-identification`, grupo `administration`, título ya implementado «Datos operacionales» y autorización server-side `operational.manage`.
- `UsersAdministrationStore` identifica a los usuarios promovidos existentes. **VERIFIED:** la proyección de Users promovidos es responsabilidad separada (`users-runtime` en la composición durable histórica); `users-support` contiene las proyecciones de apoyo como Profiles/ADA Access y, cuando se configura así, los documentos operacionales. No afirmar que los usuarios promovidos nacen en `users-support`.
- Source durable conserva los datos publicables y las versiones históricas; Cosmos sirve las proyecciones operacionales. Las aplicaciones y workers **no** deben consultar Blob como superficie de consumo.

## Contrato de dominio CURRENT — congelar durante la siguiente iteración

```text
AREA             mina | planta
GROUP            1 | 2 | 3 | 4
POSITION         id estable generado por backend; label editable; active boolean
ASSIGNMENT       user_id + area_id? + position_id? + group_id?
```

- Los tres campos de asignación son opcionales (`null` permitido) sin obligación de completar datos ficticios. No crear un cuarto campo «operational scope» por la mención histórica: la necesidad actual está cubierta por Área = Mina/Planta; **INFERRED, sujeto a nuevo requisito**.
- Un cargo tiene identificador inmutable; su etiqueta puede editarse; puede desactivarse, pero no eliminarse retrospectivamente del catálogo. Etiquetas únicas sin distinguir mayúsculas/minúsculas.
- El backend solo admite asignaciones a usuarios ya promovidos. Para asignar un cargo **nuevo o diferente**, exige catálogo Source proyectado a la revisión actual y cargo activo. Retener un cargo previamente asignado permite editar otros campos aun cuando esté desactivado o pendiente una proyección posterior del catálogo; no asignarlo a un usuario nuevo en ese estado.
- Área y grupo son referencias fijas en el dominio actual y se presentan como información, no como catálogos editables.
- La autorización de gestión (`operational.manage`) **no** otorga por sí sola permisos de sesión al usuario asignado. Identidad/promoción y Access/Profile conservan sus fronteras.

## Persistencia y proyección CURRENT

| Superficie | Estado constatado | Contrato |
|---|---|---|
| Source de catálogo | CURRENT / VERIFIED STATIC | `SourceKey('ada-operational-catalog')`; revisiones publicadas e historial. |
| Source individual | CURRENT / VERIFIED STATIC | `SourceKey('ada-operational-user:' + user_id)`; uno por usuario, independiente y con concurrencia optimista. |
| Codec Source | CURRENT / VERIFIED STATIC | Recurso `operational/data.json.gz`; `schema_version=1`; clases `catalog` / `assignment`. |
| Proyección de catálogo | CURRENT / VERIFIED STATIC | Documento `ada_operational_catalog_projection` con áreas, grupos y cargos; revisión exacta de Source. |
| Proyección individual | CURRENT / VERIFIED STATIC | Documento `ada_operational_assignment_projection`, con IDs y `source_release_id`, partición por SourceKey; no modificar otros registros de `users-support`. |
| Persistencia Cosmos | CURRENT / VERIFIED STATIC | `CosmosOperationalProjectionStore`, CAS mediante ETag y reintentos acotados; contenedor inyectable. Las pruebas usan `users-support`; la configuración física productiva no queda demostrada por tests unitarios. |
| Snapshot operacional consolidado | **PLANNED / NOT IMPLEMENTED** | No confundir con los Source individuales actuales. Shape, cobertura y política de actualización requieren cierre de contrato. |
| Registro de eventos Cosmos por cada cambio | **UNVERIFIED / NO CONTRACT** | Actualmente existe un documento de proyección **vigente** por SourceKey. Una actualización de documento Cosmos no constituye automáticamente un evento histórico independiente. |

La publicación del Source y la proyección Cosmos son pasos distintos, sin transacción distribuida. Una publicación exitosa seguida de error de proyección debe poder reintentarse desde Source durable. Las pruebas actuales ejercitan estos casos con dobles locales; no extrapolar a infraestructura Azure.

## Manager CURRENT frente a UX DECIDED

**CURRENT — interfaz en Git:** título general «Datos operacionales», pestañas de superficie «Configuración» / «Estado y trazabilidad» y, dentro de Configuración, «Asignaciones» antes de «Cargos». Existen modales, búsqueda/paginación, guardado por Source, proyección y reintentos.

**DECIDED — UX objetivo todavía NO implementada:** la primera pestaña interna será **Datos operacionales**, seguida a su lado por **Asignación**. En la primera se crean/editan/desactivan cargos; Mina/Planta y grupos 1–4 se muestran solo como referencias. Incorporar el estado y la trazabilidad según el patrón general Atlanticus de guardar/proyectar, sin perder observabilidad del catálogo ni de cada asignación. En Asignación se seleccionan únicamente los promovidos disponibles y se utiliza un modal; `user_id` permanece estable. Primero revisar la convención visual/funcional del Manager, luego realizar un incremento acotado. **No declarar esta nueva distribución UI como CURRENT por el mero cambio de nombre de la ruta.**

## Sesión y warmup — DECIDED / integración PLANNED

- La sesión Entra resuelve identidad y promoción por usuario; quien aún no ha sido promovido se representa como `guest` según el flujo definido para ADA. Una promoción requiere que esa persona **recargue la página** para re-resolver sesión. No convertir la caché de warmup en autoridad de promociones ni suponer que una recarga sustituye todas las garantías de revocación server-side.
- Tras resolver un usuario promovido y habilitado, la integración **PLANNED** consultará su **proyección operacional individual en Cosmos** y resolverá etiquetas mediante el catálogo operacional compartido ya proyectado. El backend implementa `assignment_for_resolved_user`, pero el wiring completo del inicio de sesión Entra con ese consumo sigue **UNVERIFIED**.
- El **warmup DECIDED** se limita a **catálogo de Profiles** (incluidos atributos de presentación del perfil) y **catálogo operacional** (cargos, áreas y grupos). **EXCLUDED:** listado de usuarios, promociones, asignaciones individuales y snapshot consolidado de usuarios. Refresco periódico configurable, valor inicial **PROPOSED: 10 minutos**; no declarar scheduler/runtime implementado.
- `UserRecord` ya admite colores opcionales individuales además de los colores propios de `ProfileDefinition`: conservar el ownership respectivo. El warmup de perfiles no autoriza a incorporar datos específicos de usuarios al cache compartido.

## Transiciones de estado

```text
BACKEND DOMAIN/CATALOG                         CLOSED / CURRENT / VERIFIED STATIC + 25 TESTS USER-REPORTED
SOURCE INDIVIDUAL + COSMOS PROJECTION           CLOSED / CURRENT / VERIFIED STATIC
PROJECTED-POSITION GATE PATCH                  CLOSED / CURRENT / INCORPORATED IN MAIN
MANAGER OPERATIONAL UI (CURRENT LAYOUT)         CURRENT / VERIFIED STATIC
NEW TWO-TAB MANAGER ORGANIZATION               PLANNED / DECIDED DESIGN
OPERATIONAL CONSOLIDATED SNAPSHOT              PLANNED / CONTRACT OPEN
SESSION OPERATIONAL RESOLUTION                 PLANNED / WIRING UNVERIFIED
PROFILES + OPERATIONAL CATALOG WARMUP           PLANNED / BOUNDARY DECIDED
RUFF I001                                       OPEN / ISOLATED CORRECTION
LIVE AZURE/ENTRA E2E                            UNVERIFIED
```

## Continuidad

Consultar `11_ADA_OPERATIONAL_DATA_ROADMAP.md` para el orden de incrementos, bloqueos, criterios de aceptación y única decisión aún necesaria sobre el snapshot. Consultar `../15_WEB_PLATFORM/14_ADA_OPERATIONAL_SESSION_AND_WARMUP.md` para la frontera de consumo en tiempo de ejecución.
