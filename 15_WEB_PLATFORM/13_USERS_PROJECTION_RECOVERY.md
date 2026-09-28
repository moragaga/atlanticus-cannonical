# Users — Approved Registry, Consistency and Special Recovery

Estado: **CURRENT / USERS RECOVERY IMPLEMENTED / CLOSED PARA FLUJO PREVIAMENTE VALIDADO; MASTER PREVIEW IMPLEMENTED, USERS REPLACE FROM MASTER NOT IMPLEMENTED; SECURITY/RECOVERY OPERACIONAL OPEN**  
Users código y pruebas históricas: corte documentado `atlanticus@208c8d6244795ba92cbe6f8e6b11e9743191367d`, decisiones `atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`. Delta exclusivo Master verificado estáticamente en `atlanticus@94f26213ca28b550baf53d8ee34e34da7538ad17`; no se reejecutaron las pruebas destructivas Users en este cierre.

## Alcance Users ya cerrado anteriormente

`USERS-PROJECTION-RECOVERY-001/003` incorporó captura, validación, RESTORE estricto y REPLACE desde snapshots aprobados. `USERS-PROJECTION-WEB-004/005/006/007` integró y refinó **Proyección de usuarios** dentro del Manager normal, con dos pestañas: **Crear respaldo** y **Proyectar usuarios**. El último correctivo de ese frente ajustó referencias de aprobación, limpieza de formulario tras captura, metadatos/fechas de respaldos y selector con estilos Atlanticus. Es una pantalla del Manager, no la página externa Master ni una autorización de servicio equivalente.

## Contrato CURRENT de dominio y persistencia — no modificado por Master

- `UserRecord` conserva `user_id`, `issuer`, `subject_id`, `profile_key`, `enabled` y atributos generales; no contiene cargo/área/grupo ADA.
- `UsersRegistryStore` en Blob es registro durable bajo `<application_namespace>/users/users.json.gz`. Puede contener candidatos: registro **no** significa aprobación.
- `CosmosUsersStore` mantiene promovidos en `users-runtime`, separado de `users-support` (Profiles/Access). No reconstruir un contenedor `users-runtime` entero por conveniencia.
- El snapshot aprobado es inmutable y contiene `application_key`, `identity_realm`, `origin_environment`, referencia del operador, UTC, conjunto exacto de promovidos y digest de contenido. El digest no depende del ETag físico interambientes.
- `BlobApprovedUsersSnapshotStore` almacena snapshots y catálogo; los before-images de REPLACE y eventos de auditoría son artefactos distintos y durables.
- El ámbito es de **aplicación**, no `tool_namespace`. Distintas herramientas solo comparten usuarios si consumen exactamente el mismo registro lógico y proyección; compartir cuenta Storage no basta.
- Users UI es una `ManagerEntry` con permiso `users.manage`, no `ManagerModule` Source/Projection sintético. Las acciones sensibles se autorizan en servidor con el principal normal.

## Contratos CURRENT del servicio

Código vigente de referencia:

```text
web/capabilities/users/core/src/atlanticus/web/users/recovery.py
web/capabilities/users/core/src/atlanticus/web/users/web/projection_workflow.py
web/capabilities/users/core/src/atlanticus/web/users/web/projection.py
web/capabilities/users/blob/src/atlanticus/web/users/blob/recovery.py
web/capabilities/users/cosmos/src/atlanticus/web/users/cosmos/store.py
web/compositions/users-manager/src/atlanticus/web/compositions/users_manager/composition.py
scopes/ada/web/application/ada-generic-application/src/ada/web/application/generic/manager_deployment.py
```

- `UsersAdministrationService`: `discover`, `promote`, `update`.
- `UsersApprovedRecoveryService`: `preview_capture`, `capture`, `validate`, `restore`, `validate_replace`, `replace_approved`.
- `preview_capture` compara Registry contra **todos** los promovidos y excluye candidatos. `capture` comprueba otra vez y persiste snapshot inmutable solo tras confirmación.
- `validate` es lectura: compara snapshot, registro y proyección e informa estados como `MISSING`, `DIFFERENT`, `IDENTITY_CONFLICT`, `UNEXPECTED` y `PROFILE_UNAVAILABLE`; cambios manuales en Cosmos no son aprobación.
- RESTORE estricto solo completa faltantes cuando lo permiten las precondiciones, con confirmación y mantenimiento declarados. No es REPLACE.
- `validate_replace` determina crear, modificar, eliminar, conservar, descartar candidatos y reemplazar registro; bloquea conflictos identidad/Profiles.
- `replace_approved` exige digest actual, confirmación, afirmaciones de mantenimiento y revisión de sesiones/revocaciones, ETags por usuario, before-image inmutable y auditoría `started/completed/failed` (modo REPLACE, esquema 2). No hay transacción distribuida Blob/Cosmos ni rollback automático.
- El workflow Web vuelve a validar snapshots/estado antes de escribir. Confirmación visual no prueba aislamiento real ni revocación efectiva.
- La composición durable del Manager deriva `identity_realm` del único `issuer` entre promovidos; **si no existen promovidos o hay varios emisores, ese provider no resuelve el ámbito**. Master no debe reutilizarlo automáticamente en destino vacío.

## Evidencia histórica Users, estrictamente delimitada

| Evidencia del hito Users anterior | Estado | Alcance |
|---|---|---|
| Pruebas unitarias REPLACE iniciales | VERIFIED USER-REPORTED HISTÓRICO | Selección de 50 tests aprobados. |
| Docker real REPLACE | VERIFIED USER-REPORTED HISTÓRICO | Un promovido modificado, uno inesperado eliminado y un candidato descartado; final `match`. |
| Before-image y auditoría | VERIFIED USER-REPORTED HISTÓRICO | Registry anterior de dos entradas, Cosmos previo de dos promovidos; eventos `started/completed`, una actualización y una eliminación. |
| Captura después de REPLACE | VERIFIED USER-REPORTED HISTÓRICO | Nuevo snapshot de un promovido con digest igual al original; `validate` posterior `match`. |
| Manager Users Projection visual | VERIFIED USER-REPORTED HISTÓRICO | Captura modal, fecha/metadatos y refinamientos visuales aceptados. |
| Tests seleccionados tras correctivo 007 | VERIFIED USER-REPORTED HISTÓRICO | Core/Blob/composición/Manager/ADA Generic antes del commit `208c8d6`. |
| Inspección estática histórica `208c8d6` | VERIFIED STATIC | Servicio, composición y UI publicados. |
| Ruff final/CI global/REPLACE invasivo desde navegador | UNVERIFIED | No certificado para el incremento actual. |
| Interrupción a mitad de REPLACE/revocación de sesiones | UNVERIFIED | Sigue siendo gate productivo. |

No reetiquetar estas pruebas como suite ejecutada en `94f2621` por actualizar este documento.

## Delta Master Projection actual — no sustituye Users Recovery

El planner Master implementado en `74f9107` consulta el catálogo de snapshots desde `ConfigurationManagerStores.users_snapshot_ids`, **fuera** de los seis pares ordinarios, y expone los estados `CATALOG_UNAVAILABLE`, `SNAPSHOT_MISSING`, `PROFILES_PENDING`, `SNAPSHOT_SELECTION_REQUIRED`. `MasterProjectionPlan.to_dict()` indica `operation: users.replace` pero fija **`executable: false`**.

La página independiente `/master-projection` está implementada por `e217754`, con material y autenticación de servicio externa, y **muestra** el plan en modo de solo lectura. Su material ZIP declara también `users.replace`, pero **no** existe ejecución Users REPLACE desde Master 001C. El usuario informó acceso real local en una distribución de prueba; esto **no** acredita transporte interambientes, selección/aplicación de snapshot ni seguridad de recuperación productiva.

La excepción de credenciales Master frente a baseline pre-Manager/Entra es un **CONFLICT contractual OPEN**, y la resolución de `identity_realm` para destino vacío permanece pendiente. No habilitar Users REPLACE como efecto implícito de implementar `projection.apply` para los seis módulos ordinarios de 001D.

## Refinamientos y reemplazos conservados

- **SUPERSEDED:** descripciones históricas de Users Recovery como no implementado y de su UI Manager como inexistente.
- **SUPERSEDED:** variables adicionales `ADA_USERS_RECOVERY_IDENTITY_REALM` / `ADA_USERS_RECOVERY_ENVIRONMENT` retiradas del wiring del Manager; no reintroducirlas por Master.
- **SUPERSEDED:** UI antigua de dos columnas simultáneas, terminología mixta RESTORE/REPLACE, confirmación mecanografiada, selector sin contexto y referencias espaciadas rechazadas.
- **CURRENT:** pestañas separadas, términos en español, comparación/previsualización, modal explícito, metadatos y estilos Atlanticus.
- **SUPERSEDED respecto a Master únicamente:** afirmación «página externa Master no implementada». 001C ya tiene página/login/preview; **Users REPLACE externo sigue no implementado**.

## OPEN / condiciones para producción y continuidad

1. `SECURITY`: mantenimiento efectivo, control de concurrencia operacional, reevaluación/revocación real de sesiones. Booleanos/UI no sustituyen controles.
2. `RECOVERY`: interrupción real tras escrituras parciales, auditoría de fallos, revalidación y reintento. No prometer atomicidad ni rollback.
3. `QUALIFICATION`: Ruff final/CI/Entra y operación invasiva desde navegador.
4. `INTERAMBIENTES`: transporte autorizado de snapshot y bootstrap destino sin promovidos, compatibilidad issuer/subject; nunca promocionar candidatos automáticamente.
5. `MASTER-USERS-REPLACE`: requiere un incremento/gate específico después de resolver seguridad e identidad de destino; **no** forma parte de MASTER-001D inicial para seis proyecciones ordinarias.

Users Recovery continúa **CLOSED** solo para su flujo anterior validado en laboratorio, con gates productivos explícitos. No reabrir ese cierre para estética ni introducir adapters legacy sin finding.
