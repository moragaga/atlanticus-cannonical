# Manager — Bootstrap and Access

Estado: **CURRENT — MANAGER USERS RECOVERY EN SU ALCANCE; MASTER EXTERNO PREVIEW+APPLY INDIVIDUAL IMPLEMENTADOS; USERS REPLACE DESDE MASTER BLOCKED**.  
Master contrastado en `atlanticus@9c6daffd04b9c249f75a55b6cdb9b44e6d92a795`; histories de Users conservadas según sus propios cortes anteriores. Decisions Markdown consultadas en `atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`; DOCX de identidad no revalidados exhaustivamente en este cierre.

## Manager e Identity CURRENT

`BOOTSTRAP ACCESS != MANAGER ACCESS`: autenticación Identity no otorga permisos de configuración. Los módulos/entries y acciones de Manager dependen de `ManagerPrincipal`, `access_keys` y autorización servidor. ADA incluye `users.manage`, `profiles.manage`, `access.manage`, `navigation.manage`, `tools.manage`, `kpis.manage`. La identidad local está prevista solo para local; producción necesita provider de identidad aportado por el host.

Atlanticus Manager sigue siendo genérico y no se acopla a ADA. `/manager` es Home real; cards/sidebar proceden del registro común tras comprobar visibilidad y el workflow administrativo se mantiene separado del formulario de cada dominio, según las reglas aprobadas del Manager del 2026-09-02. El código Master no reemplaza ni autoriza ese workflow.

## Users CURRENT — contrato conservado

```text
<application_namespace>/users/users.json.gz   Blob registry durable; admite candidatos
approved snapshots                            Blob capturas inmutables autorizadas
replace before-image + audit                  Blob evidencia de sustitución
users-runtime                                 Cosmos promovidos
users-support                                 Cosmos Profiles + ADA Access; no promovidos
```

`UsersAdministrationService` mantiene `discover/promote/update`; `UsersApprovedRecoveryService` mantiene `preview_capture/capture/validate/restore/validate_replace/replace_approved`. `discover` no equivale a aprobación; la restauración/sustitución parte únicamente de snapshots **expresamente aprobados**. No se promete transacción distribuida ni rollback. Las operaciones REPLACE usan before-image y auditoría existentes y exigen precondiciones actualizadas.

La página ordinaria del Manager **Proyección de usuarios** requiere `users.manage` y presenta dos pestañas **Crear respaldo** / **Proyectar usuarios**. Es una `ManagerEntry`, no un Source/Projection séptimo ni la superficie Master. El registro de Users está bajo `application_namespace`, no `tool_namespace`. El provider durable actual deduce `identity_realm` del único issuer entre promovidos; un destino sin promovidos **no** queda resuelto por ese mecanismo.

**VERIFIED USER-REPORTED HISTÓRICO:** flujo local de REPLACE con before-image, auditoría y estado final `match`; aceptación visual de Manager Users. No atribuir esas pruebas al HEAD Master de este cierre. **UNVERIFIED:** fallo parcial real/revocación efectiva y operación invasiva desde navegador/productivo.

## Master externo — separación y controles CURRENT

Master vive en la composición Web existente; el usuario del ZIP protegido no pasa a ser `ManagerPrincipal`, no hereda `users.manage` ni permite ingresar automáticamente a `/manager`. Código:

```text
tooling/distribution/web/starter/ada/tooling/master_projection.py
tooling/distribution/web/starter/ada/src/application/master_projection/{material,reader}.py
scopes/ada/web/application/ada-generic-application/src/ada/web/application/generic/master_projection/{plan,composition,apply,web}.py
```

El lector usa `ADA_MASTER_PROJECTION_MATERIAL_PATH` opcional/external y estados `ABSENT/PRESENT/INVALID`. El ZIP AES-256/scrypt vincula usuario de servicio, aplicación, ambiente y acciones. Las sesiones Master duran 900 segundos desde emisión y se invalidan al sustituir/desaparecer el material; login y POST requieren CSRF. Logout es **POST** `/master-projection/logout` con CSRF. `GET/POST /master-projection` y dicha ruta de logout son las **únicas excepciones exactas** al middleware ordinario; el registro Master precede a Identity/Navigation. No crear bypass de prefijo.

Con `projection.preview` se ve el planner read-only de seis dominios y el estado Users separado. Con **`projection.apply` y executor presente**, la página ofrece preparar/confirmar la proyección de un dominio `NEVER_PROJECTED` u `OUTDATED` con selección de target exacto + nonce en sesión firmada, CSRF, confirmación humana y reinspección al confirmar. El backend valida otra vez el target y verifica el resultado persistido. Si el target ya está alineado devuelve `ALREADY_CURRENT`. La operación de escritura Master nunca edita/publica Source.

**Users Master REPLACE sigue no ejecutable:** `users.replace` figura como acción declarada en material, pero el planner marca `executable: false` y no existe un controller de sustitución desde Master. No equiparar esto a la función REPLACE ya existente del Manager.

## Qualification y tensiones

**VERIFIED USER-REPORTED 001D.2:** 50 pruebas seleccionadas PASS, Ruff/diff check PASS. **VERIFIED USER-REPORTED Docker 001D.3 (`ca3ee508`):** en navegador el usuario ingresó a Master, observó inicialmente seis Source missing, publicó una modificación de Navigation, preparó/confirmó Apply y Manager mostró Navigation proyectado/sincronizado al recargar. Solo Navigation se ensayó end-to-end de este modo. El correctivo local de Starter 001D.4 fue publicado en `9c6daffd` y verificado después en distribución separada, sin volver a desplegar esta versión nueva en Docker.

**TENSIÓN DOCUMENTADA / DECISIÓN A CONTRASTAR:** la canonical histórica recoge una baseline Entra para superficies pre-Manager y Master usa credencial independiente de servicio. Los DOCX pertinentes de decisiones no se auditaron exhaustivamente en este cierre; no afirmar que la tensión esté formalmente resuelta ni que haya un conflicto documental confirmado sin leerlos. La validación local no autoriza producción.

**OPEN:** custodia/warmup productivo del ZIP, rotación/revocación real, Azure/Entra, multiworker, cinco dominios restantes de Master y gates de Users destino vacío, issuer, revocación y fallo parcial. No introducir variables redundantes históricas de Users, código legacy ni una superficie Manager duplicada.
