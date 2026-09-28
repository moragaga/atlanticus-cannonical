# Users — Approved Registry, Consistency and Special Recovery

Estado: **CURRENT / IMPLEMENTED / CLOSED PARA EL FLUJO VALIDADO; SECURITY Y RECOVERY OPERACIONAL OPEN**  
Corte de implementación (VERIFIED STATIC): `moragaga/atlanticus:main@208c8d6244795ba92cbe6f8e6b11e9743191367d`.  
Decisiones consultadas: `moragaga/atlanticus-decisions:main@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`, reglas globales aprobadas del Manager.  
Base documental anterior: `moragaga/atlanticus-cannonical:main@e49901fb3ceef5431edbde1d0dcbb29fc3502855`.  
Ejecuciones Docker/UI: **VERIFIED USER-REPORTED** en este hito; no equiparar con CI del commit ni con producción.

## Alcance cerrado en este hito

`USERS-PROJECTION-RECOVERY-001/003` incorpora captura, validación, RESTORE estricto y REPLACE desde snapshots aprobados. `USERS-PROJECTION-WEB-004/005/006/007` integra y refina **Proyección de usuarios** dentro del Manager normal, con dos pestañas: **Crear respaldo** y **Proyectar usuarios**. El último correctivo ajusta las referencias de aprobación, limpia el formulario después de capturar, presenta fechas/metadatos de respaldos y alinea el selector con estilos Atlanticus. `USERS-PROJECTION-WEB-007` corresponde al corte de código arriba indicado.

La pantalla del Manager **no** es la futura página externa Master Projection. No fusionar ambas funciones ni sus autorizaciones.

## Contrato CURRENT de dominio y persistencia

- `UserRecord` conserva `user_id`, `issuer`, `subject_id`, `profile_key`, `enabled` y sus atributos generales; no contiene cargo/área/grupo ADA.
- `UsersRegistryStore` en Blob es el registro durable de usuarios bajo `<application_namespace>/users/users.json.gz`. Puede contener candidatos: presencia en registro **no** equivale a aprobación.
- `CosmosUsersStore` en `users-runtime` conserva promovidos por documento, separado de `users-support` (Profiles/Access). No reconstruir `users-runtime` entero.
- El snapshot aprobado es inmutable e incluye `application_key`, `identity_realm`, `origin_environment`, identificación y referencia del operador, fecha UTC, conjunto exacto de promovidos y digest de contenido/integridad. El digest de contenido no depende de un ETag físico interambientes.
- `BlobApprovedUsersSnapshotStore` conserva snapshots; el catálogo usa propiedades/listado de Blob para fechas y la selección descarga el artefacto elegido. Los respaldos de REPLACE (`before-image`) y la auditoría son artefactos distintos, también durables.
- El ámbito de usuarios es de **aplicación**, no de `tool_namespace`. Herramientas distintas pueden compartir usuarios si consumen de forma consistente el **mismo registro lógico y proyección**; compartir sólo una cuenta Storage no crea por sí mismo usuarios globales.
- La UI de Users es `ManagerEntry`, no `ManagerModule` sintético con Source/Projection. Las acciones se autorizan en servidor y usan el principal existente y `users.manage`.

## Contratos CURRENT del servicio

Código de referencia:

```text
web/capabilities/users/core/src/atlanticus/web/users/recovery.py
web/capabilities/users/core/src/atlanticus/web/users/web/projection_workflow.py
web/capabilities/users/core/src/atlanticus/web/users/web/projection.py
web/capabilities/users/blob/src/atlanticus/web/users/blob/recovery.py
web/capabilities/users/cosmos/src/atlanticus/web/users/cosmos/store.py
web/compositions/users-manager/src/atlanticus/web/compositions/users_manager/composition.py
scopes/ada/web/application/ada-generic-application/src/ada/web/application/generic/manager_deployment.py
```

- `preview_capture` compara el registro con **todos los promovidos** y excluye candidatos. `capture` vuelve a comprobar la vista previa y persiste un snapshot inmutable únicamente tras confirmación.
- `validate` es de lectura, compara snapshot/registro/proyección e informa `MISSING`, `DIFFERENT`, `IDENTITY_CONFLICT`, `UNEXPECTED` y `PROFILE_UNAVAILABLE` según corresponda. No interpreta cambios directos de Cosmos como aprobación.
- RESTORE estricto aplica sólo cuando las precondiciones permiten completar usuarios faltantes sin sobrescribir conflictos; requiere confirmación y mantenimiento declarados. No es sinónimo de REPLACE.
- `validate_replace` produce plan: crear, modificar, eliminar, conservar, descartar candidatos y reemplazar registro; bloquea conflictos de identidad o Profiles.
- `replace_approved` requiere digest vigente, confirmación, afirmaciones de mantenimiento y revisión de sesiones/revocaciones, ETags por usuario, `before-image` inmutable y auditoría `started/completed/failed` (modo REPLACE, esquema 2). Reconcilia el registro con el conjunto exacto del snapshot, elimina promovidos inesperados y actualiza/crea según plan. No hay transacción distribuida Blob/Cosmos ni rollback automático.
- El workflow de la pantalla vuelve a validar snapshot y estado antes de escribir; las confirmaciones visuales **no** prueban que haya aislamiento real ni revocación efectiva de sesiones.
- En la composición durable actual, el `identity_realm` se deriva del único `issuer` entre promovidos; **si no hay promovidos o hay varios emisores, esta composición no resuelve el ámbito**. Master Projection deberá tratar su propio bootstrap inicial sin asumir que este provider del Manager funciona en un destino vacío.

## Qualification delimitada

| Evidencia | Estado | Alcance |
|---|---|---|
| Pruebas unitarias del incremento REPLACE | VERIFIED USER-REPORTED | Selección inicial de 50 tests aprobados. |
| Ejecución Docker real REPLACE | VERIFIED USER-REPORTED | Un promovido modificado, un inesperado eliminado y un candidato descartado; resultado final `match`. |
| `before-image` y auditoría | VERIFIED USER-REPORTED | Registro previo 2 entradas, Cosmos previo 2 promovidos, eventos `started` y `completed`, 1 update, 1 delete, sin creates. |
| Captura posterior a REPLACE | VERIFIED USER-REPORTED | Nuevo snapshot de 1 promovido, digest igual al original, `validate` posterior `match`. |
| Manager Users Projection y captura visual | VERIFIED USER-REPORTED | Captura desde modal y refinamientos visuales validados por el usuario; último resultado «quedó ok». |
| Tests seleccionados tras correctivo 007 | VERIFIED USER-REPORTED | Selección de tests Core/Blob/composición/ADA Manager/ADA Generic al 100% antes de consolidar el commit 208c8d6. |
| Inspección estática del commit 208c8d6 | VERIFIED STATIC | Archivos de servicio, composición y UI presentes. |
| Ruff final, CI monorepo, operación invasiva desde navegador | UNVERIFIED | No se aportó evidencia final de estas calificaciones. |
| Interrupción real a mitad de REPLACE, recuperación y revocación de sesiones | UNVERIFIED | Fuera del flujo feliz del laboratorio; gate productivo pendiente. |

## Refinamientos y reemplazos

- **SUPERSEDED:** descripción previa de `USERS-PROJECTION-RECOVERY` como `NOT IMPLEMENTED` y de su UI como inexistente.
- **SUPERSEDED:** variables adicionales `ADA_USERS_RECOVERY_IDENTITY_REALM` / `ADA_USERS_RECOVERY_ENVIRONMENT` para la página del Manager; se retiraron del wiring. La aplicación/tooling no debe añadir configuración redundante por herramienta.
- **SUPERSEDED:** la UI de dos columnas simultáneas, terminología mixta `RESTORE/REPLACE`, confirmación mecanografiada, selector sin contexto y referencias con espacios rechazadas por el workflow de captura.
- **CURRENT:** pestañas separadas, nombres comprensibles en español, previsualización/comparación, modal explícito, metadatos del respaldo y CSS propio alineado a Users Administration.

## OPEN / condiciones antes de habilitar uso productivo

1. `OPEN / SECURITY`: mantenimiento efectivo, control de concurrencia operacional y reevaluación/revocación de sesiones; los booleanos operacionales no sustituyen controles reales.
2. `OPEN / RECOVERY`: probar interrupción real tras escrituras parciales, auditoría de fallo, revalidación y reintento; no afirmar atomicidad ni rollback.
3. `OPEN / QUALIFICATION`: Ruff final, CI/monorepo, producción Entra y ejecución de RESTORE/REPLACE invasivo **desde la UI**.
4. `OPEN / INTERAMBIENTES`: transporte autorizado de snapshots y bootstrap en destino sin promovidos; no asumir equivalencia entre distintos emisores Entra ni promover automáticamente candidatos.
5. `OPEN / MASTER-PROJECTION`: página externa y material de acceso independiente. Consultar `06_PRE_MANAGER_BOOTSTRAP_SURFACE.md`.

Este incremento se considera **CLOSED** para el flujo respaldar/comparar y REPLACE validado en laboratorio, con pendientes productivos explícitos. No reabrir Users por estilo ni introducir adaptadores legacy sin finding real.
