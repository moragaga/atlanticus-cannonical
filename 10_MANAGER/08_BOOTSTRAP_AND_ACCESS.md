# Manager — Bootstrap and Access

Estado: **CURRENT / USERS RECOVERY + MANAGER USERS PROJECTION IMPLEMENTED / EXTERNAL MASTER PREVIEW IMPLEMENTED / MASTER APPLY PLANNED**  
Users/Manager: evidencias históricas delimitadas en los documentos vigentes. Master: inspección estática `atlanticus@94f26213ca28b550baf53d8ee34e34da7538ad17`; decisions `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e` consultadas parcialmente. No extender ensayos de laboratorio a producción.

## Manager e Identity CURRENT

`BOOTSTRAP ACCESS != MANAGER ACCESS`. Autenticarse en Identity no otorga permisos Manager: autorización `ManagerModule`/`ManagerEntry` depende de principal y `access_keys`; operaciones sensibles se autorizan nuevamente en servidor. `is_local` no es permiso general. Atlanticus Manager es genérico; ADA compone `profile_key -> access_keys` y sus módulos.

Las claves del Manager ADA incluyen `users.manage`, `profiles.manage`, `access.manage`, `navigation.manage`, `tools.manage` y `kpis.manage`. `/manager` continúa como Home real; Home/sidebar derivan del registro autorizado común; páginas de dominio no deben replicar interfaz global. Existe `LocalIdentityProvider` en local; la identidad productiva exige un `IdentityProvider` apropiado aportado por el host y no queda validada por esta prueba de Master.

## Users y recuperación CURRENT

```text
<application_namespace>/users/users.json.gz   Blob registro durable; incluye posibles candidatos
approved snapshots                            Blob capturas inmutables de promovidos
replace before-image + audit                  Blob evidencias de sustitución
users-runtime                                 Cosmos documentos de promovidos
users-support                                 Cosmos Profiles + ADA Access, NO promovidos
```

`promote/update` escriben Blob antes que Cosmos y no tienen transacción distribuida. `discover` no constituye reconstrucción de seguridad. `UsersApprovedRecoveryService` integra captura, validación, RESTORE estricto y REPLACE desde snapshot expresamente aprobado con ETags, before-image y auditoría de started/completed/failed.

La página interna **Proyección de usuarios** es una `ManagerEntry` con `users.manage`, pestañas **Crear respaldo** y **Proyectar usuarios**, historial, comparación/selección y modal. No registrar Sources/Projections sintéticos para esta Entry. El registro utiliza `application_namespace` (no `tool_namespace`); compartir una cuenta Storage no equivale automáticamente a usuarios compartidos. Variables redundantes históricas de Users Recovery quedaron SUPERSEDED y no deben resucitar.

El provider durable del Manager deriva `identity_realm` de un `issuer` único entre promovidos; no utilizarlo para justificar un destino sin promovidos. **VERIFIED USER-REPORTED en su hito previo:** ejecución de REPLACE en laboratorio con modificación/eliminación/descarte y estado final `match`, before-image, auditoría, captura posterior y UI aceptada. **UNVERIFIED:** operación destructiva en browser, interrupción real/reintento, revocación efectiva y operación productiva.

## Master Projection externo — CURRENT PREVIEW, NO MANAGER

Master se aloja en la Web existente, pero no es una pestaña ni una autorización del Manager. El usuario de servicio almacenado en el material protegido **no se convierte** en `ManagerPrincipal`, no recibe `users.manage` y no habilita automáticamente ninguna página `/manager`.

Implementaciones de referencia:

```text
tooling/distribution/web/starter/ada/tooling/master_projection.py
tooling/distribution/web/starter/ada/src/application/master_projection/{material,reader}.py
scopes/ada/web/application/ada-generic-application/src/ada/web/application/generic/master_projection/{plan,composition,web}.py
web/capabilities/identity/core/src/atlanticus/web/identity/module.py
```

**CURRENT:** la URL `/master-projection` maneja `ABSENT/PRESENT/INVALID` y habilita login independiente solo con material íntegro y credenciales correctas. Solo `/master-projection` y `POST /master-projection/logout` están exceptuadas por **match exacto** del middleware Identity; el middleware Master se registra antes de Navigation para impedir el `AccessContextError` que reveló la primera prueba de distribución. Ruta desconocida bajo el prefijo sigue autenticación ordinaria. Sesión Master: 900 s fijos, CSRF para POST, fingerprint para revocar sesiones al sustituir material y lectura del planner tras autorización.

**CURRENT:** se observan seis proyecciones ordinarias y estado Users separado desde un planner solo lectura; no se presentan acciones de Apply/REPLACE. El formato ZIP declara `projection.preview`, `projection.apply`, `users.replace`, pero hoy **solo** hay controller read-only de la primera. Declaración de permisos ≠ controlador implementado.

**VERIFIED USER-REPORTED LOCAL:** `BUILT_UNQUALIFIED` de 67 wheels, `SYNCED`, HTTP 200 en estados sin/con ZIP y login real exitoso. **OPEN:** full test rerun posterior al último commit, visualización expresa del plan y logout/relogin manual, qualification desde checkout limpio, warmup/cloud productivo. Consultar `../15_WEB_PLATFORM/06_PRE_MANAGER_BOOTSTRAP_SURFACE.md`.

## Restricciones de seguridad y conflicto de decisión

1. No convertir la exención Master en bypass general de Identity o Navigation ni en elevación de Manager.
2. El material ZIP es externo a repo/Starter y la contraseña no se guarda en claro; ruta actual opcional `ADA_MASTER_PROJECTION_MATERIAL_PATH`. La custodia/warmup productiva no está certificada.
3. El acceso independiente Master es una **excepción técnica implementada**; la baseline histórica que exigía Entra para toda superficie pre-Manager permanece **OPEN / CONTRACT CONFLICT** hasta revisar formalmente decisiones pertinentes y delimitar controles productivos. Las reglas globales Markdown del Manager no bastan para declarar resuelto el conflicto de identidad.
4. Registry de Users puede contener candidatos; solo snapshots aprobados gobiernan recuperación. Cosmos no es authority durable del registro y ETag físico no es identidad portable.
5. Confirmaciones de mantenimiento/revocación en UI no son aislamiento/revocación operacional ejecutados. Cada futura escritura requiere validación servidor y precondiciones actuales.
6. No introducir cargo/área/grupo ADA en Core Users genérico ni una nueva superficie Manager para encubrir Master.

## Siguiente frontera acotada

`MASTER-PROJECTION-001D`: inspección/debate de `projection.apply` para los seis pares ordinarios utilizando servicios actuales, sin código antes de consenso y sin incluir Users REPLACE. Los pendientes productivos de Users, Entra y warmup siguen diferenciados. Para detalle de Users consultar `../15_WEB_PLATFORM/13_USERS_PROJECTION_RECOVERY.md`.
