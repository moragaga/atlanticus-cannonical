# Users — Approved Registry, Consistency and Special Recovery

Estado: **CURRENT — USERS RECOVERY/MANAGER USERS PROJECTION IMPLEMENTADOS EN SU ALCANCE PREVIO; MASTER 001D ORDINARY APPLY IMPLEMENTADO; USERS REPLACE DESDE MASTER NO IMPLEMENTADO/BLOCKED**.  
Users histórica según `atlanticus@208c8d6244795ba92cbe6f8e6b11e9743191367d`; delta Master contrastado en `atlanticus@9c6daffd04b9c249f75a55b6cdb9b44e6d92a795`. Decisions históricas `atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`, sin auditoría exhaustiva de DOCX Entra/pre-Manager. La validación Master no reejecutó las pruebas destructivas Users.

## Contrato Users CURRENT — inalterado por Master

- `UserRecord` contiene identidad, `issuer`, `subject_id`, `profile_key`, `enabled` y atributos generales; **no** incorpora cargo/área/grupo específicos de ADA.
- `UsersRegistryStore` en Blob es registro durable bajo `<application_namespace>/users/users.json.gz`; puede contener candidatos. **Registry no es aprobación**.
- `CosmosUsersStore` mantiene promovidos en `users-runtime`, separado de `users-support` (Profiles/ADA Access). No reemplazar un contenedor Cosmos entero por conveniencia.
- Snapshots aprobados inmutables contienen ámbito de aplicación, `identity_realm`, ambiente de origen, operador/UTC, conjunto exacto de promovidos y digest de contenido. El digest no depende del ETag físico entre ambientes.
- Before-images de REPLACE y eventos de auditoría son artifacts durables distintos. Sharing de cuenta Storage no implica sharing del registro lógico; Users está bajo `application_namespace` y **no** `tool_namespace`.
- La UI **Proyección de usuarios** sigue siendo `ManagerEntry` autorizada mediante `users.manage`; pestañas **Crear respaldo** y **Proyectar usuarios**. No inventar un par Source/Projection Users ni conceder permisos Manager a Master.

## Servicios CURRENT existentes

```text
web/capabilities/users/core/src/atlanticus/web/users/recovery.py
web/capabilities/users/core/src/atlanticus/web/users/web/projection_workflow.py
web/capabilities/users/core/src/atlanticus/web/users/web/projection.py
web/capabilities/users/blob/src/atlanticus/web/users/blob/recovery.py
web/capabilities/users/cosmos/src/atlanticus/web/users/cosmos/store.py
web/compositions/users-manager/src/atlanticus/web/compositions/users_manager/composition.py
scopes/ada/web/application/ada-generic-application/src/ada/web/application/generic/manager_deployment.py
```

`UsersAdministrationService`: `discover/promote/update`. `UsersApprovedRecoveryService`: `preview_capture/capture/validate/restore/validate_replace/replace_approved`. La captura compara Registry con promovidos, excluye candidatos y guarda snapshots solo tras confirmación y validación. `validate` distingue entradas faltantes, diferentes, conflictos de identidad, inesperadas o perfiles no disponibles. RESTORE estricto no es REPLACE. `validate_replace` produce comparación explícita; `replace_approved` requiere digest actual, confirmación, declaraciones de mantenimiento/revocación, ETags, before-image y auditoría started/completed/failed.

No se promete transacción distribuida ni rollback entre Blob/Cosmos. Confirmar en UI no es demostrar revocación/aislamiento operacional. El provider durable del Manager deriva `identity_realm` del único issuer entre promovidos: destino vacío o varios issuers es **un gate no resuelto**, no una justificación para defaults inventados.

## Qualification Users histórica conservada

**VERIFIED USER-REPORTED de su hito previo:** selección inicial de 50 tests; Docker local REPLACE con un promovido actualizado, inesperado eliminado y candidato descartado; estado `match`, before-image/auditoría started+completed y snapshot posterior equivalente; UI del Manager aceptada visualmente y pruebas seleccionadas antes del commit Users correspondiente. Cada evidencia corresponde a su checkpoint histórico, no a Master 9c6.

**UNVERIFIED:** operación destructiva desde navegador con fault injection, revocación real de sesiones, carrera/aislamiento y producción. No reinterpretar esa deuda como contrato nuevo de Master.

### Refinamientos UI Users históricos conservados

Las iteraciones anteriores de la pantalla del Manager corrigieron referencias de aprobación, limpieza del formulario tras captura, fechas/metadatos de respaldos y selector con estilos Atlanticus. Quedaron **SUPERSEDED** la antigua interfaz de dos columnas simultáneas, términos mezclados RESTORE/REPLACE, confirmación mecanografiada, selector sin contexto y referencias espaciadas rechazadas. La versión actual mantiene pestañas separadas, comparación y previsualización, modal explícito y términos en español. No reabrir el diseño visual desde el cierre Master.

## Master — delta 001D actual

El planner de seis dominios ordinarios (`Navigation`, `Profiles`, `Tools`, `ADA Access`, `KPI Registry`, `KPI Definitions`) informa Users separadamente con estados `CATALOG_UNAVAILABLE`, `SNAPSHOT_MISSING`, `PROFILES_PENDING`, `SNAPSHOT_SELECTION_REQUIRED`. `to_dict()` incluye `operation: users.replace` pero **`executable: false`**.

La página externa Master **ya no es exclusivamente preview**: el código 001D.1/001D.2 implementa `projection.apply` **individual y con confirmación** para los seis dominios ordinarios. Ello **no** implementa `users.replace`: la mera presencia de esa acción en el material AES-256 no constituye autorización ni endpoint de Users. 001D.3 ensayó en Docker **Navigation**, no Users.

## Refinamientos y condiciones OPEN

- **SUPERSEDED:** descripciones que consideraban inexistente la página Master o que decían que toda acción Apply desde Master era futura. La acción de los **seis pares ordinarios** sí existe; **Users Master REPLACE sigue no implementado**.
- **SUPERSEDED histórico:** variables redundantes retiradas `ADA_USERS_RECOVERY_IDENTITY_REALM` / `ADA_USERS_RECOVERY_ENVIRONMENT`; no resucitarlas.
- **CURRENT:** snapshots expresamente aprobados; separación Registry/Promoted/Approved, RESTORE/REPLACE, before-images, auditoría y frontend Manager propio.
- **BLOCKED:** Users REPLACE desde Master a destino sin promovidos; compatibilidad de issuer/subject_id, transporte aprobado, mantenimiento/revocación y recuperación tras error parcial requieren **otro incremento y autorización explícita**.
- **OPEN:** excepción de servicio Master frente a baseline histórica Entra por contrastar con decisiones DOCX antes de producción.

No abrir Users como efecto lateral de documentar 001D.1–001D.4, ni introducir código legacy.
