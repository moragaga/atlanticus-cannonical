# Manager — Bootstrap and Access

Estado: **CURRENT / USERS RECOVERY + MANAGER USERS PROJECTION IMPLEMENTED / EXTERNAL MASTER PLANNED**  
Corte: `atlanticus@208c8d6244795ba92cbe6f8e6b11e9743191367d`, `atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`. No extender pruebas del laboratorio a producción.

## Manager e Identity CURRENT

`BOOTSTRAP ACCESS != MANAGER ACCESS`. Identity no otorga permisos Manager por el solo hecho de autenticar: la autorización de un `ManagerModule`/`ManagerEntry` deriva del principal y sus `access_keys`; el servidor debe volver a verificar autorización en acciones sensibles. `is_local` no es permiso general. El Manager Atlanticus es genérico y ADA compone Access `profile_key -> access_keys` y sus módulos.

Las claves del Manager ADA incluyen `users.manage`, `profiles.manage`, `access.manage`, `navigation.manage`, `tools.manage` y `kpis.manage`. `/manager` sigue siendo Home real; Home/sidebar derivan del registro autorizado común y las páginas no deben replicar UI global. En local existe `LocalIdentityProvider`; producción exige integrar el `IdentityProvider` apropiado, no inferir Entra calificada por existir la interfaz.

## Users y recuperación CURRENT

```text
<application_namespace>/users/users.json.gz   Blob registro durable; puede tener candidatos
approved snapshots                      Blob capturas inmutables de promovidos
replace before-image + audit            Blob evidencias de sustitución
users-runtime                           Cosmos documentos de promovidos
users-support                           Cosmos Profiles + ADA Access (NO promovidos)
```

`promote/update` escriben Blob antes que Cosmos y **no** son una transacción distribuida. `discover` no es una reconstrucción de seguridad. `UsersApprovedRecoveryService` agrega captura, validación, RESTORE estricto y REPLACE invasivo desde snapshot **expresamente aprobado**, con ETags, respaldo previo y auditoría de started/completed/failed.

La página interna **Proyección de usuarios** es `ManagerEntry` bajo Administración con permiso `users.manage`. Sus pestañas **Crear respaldo** y **Proyectar usuarios** separan flujos: historial legible, fecha/detalles del respaldo, comparación de diferencias, elecciones distintas de restauración estricta y sustitución completa y confirmación mediante modal. No crear Source/Projection sintéticos para esta Entry ni suponer que una captura habilita a todos los candidatos.

Se retiraron las variables extra de Users Recovery del wiring del Manager. El registro usa `application_namespace`, no `tool_namespace`; aplicaciones/herramientas pueden compartir usuarios únicamente si el registro y proyección que consumen son realmente comunes. El provider durable actual deriva `identity_realm` del `issuer` único de promovidos: no usarlo para justificar bootstrap en ambiente sin promovidos.

**VERIFIED USER-REPORTED:** tests seleccionados, ejecución real REPLACE en laboratorio Cosmos/Azurite con 1 actualización, 1 eliminación, 1 candidato descartado y `match`; verificación de `before-image`, auditoría, captura posterior; aceptación de UI y test seleccionado tras correctivo 007. **UNVERIFIED:** operación invasiva desde UI, revocación efectiva de sesiones, interrupción real con retry, CI/Ruff final en commit y despliegue productivo.

## Master Projection: acceso excepcional FUERA de Manager

**PLANNED / NEXT.** El problema de preparación de ambientes sin Profiles/Access/Users se resuelve mediante una **página por URL independiente de `/manager`**, con credenciales y material protegido generados mediante tooling ADA existente (contrato concreto pendiente). No concede permisos Manager, no edita Sources ni instala automáticamente usuarios ficticios.

- Material **ausente**: la página informa que no hay acceso configurado actualmente y bloquea toda autenticación/acción sin producir error de arranque innecesario.
- Material **presente e íntegro**: exige autenticación de servicio antes de mostrar plan/acciones y permite proyectar configuraciones aplicables usando servicios existentes, dependencias reales y reintentos controlados. Users usa procedimiento especial a partir de snapshot aprobado.
- El material no caduca automáticamente ni se consume por un intento exitoso; la política de revocación y regeneración debe diseñarse explícitamente. No guardar contraseña en claro ni afirmar que un hash permite descifrar datos.
- La baseline antigua que pedía Entra para toda superficie pre-Manager **requiere delimitarse** frente a esta excepción de credencial propia. No sustituye ni da por implementada Entra productiva normal.

## Reglas congeladas

1. Atlanticus Users/Manager/Identity genéricos; Access y futuros cargo/área/grupo ADA permanecen fuera del Core genérico.
2. Candidatos/Guest nunca se convierten automáticamente en aprobados desde el registro.
3. Blob aprobado gobierna recuperación; Cosmos no se promueve a autoridad. ETag físico no es identidad portable.
4. UI/client/localStorage no son autoridad; las acciones sensibles necesitan autorización de servidor y precondiciones nuevas.
5. Confirmar mantenimiento/revocaciones **no equivale a ejecutarlos**: gate productivo pendiente.
6. Master es una superficie separada con autenticación independiente y dos estados según el material, no un bypass ni una nueva sección del Manager.

Para contratos/evidencia de Users, ver `../15_WEB_PLATFORM/13_USERS_PROJECTION_RECOVERY.md`. Para Master, ver `../15_WEB_PLATFORM/06_PRE_MANAGER_BOOTSTRAP_SURFACE.md`.
