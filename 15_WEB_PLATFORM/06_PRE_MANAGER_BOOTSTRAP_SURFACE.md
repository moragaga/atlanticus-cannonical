# Web Platform — Pre-Manager Bootstrap and Isolated Master Projection Surface

Estado: **001A MATERIAL CURRENT/CLOSED; 001B PLANNER CURRENT/CLOSED; 001C HTTP CURRENT/IMPLEMENTED, QUALIFICATION FINAL IN PROGRESS; 001D APPLY PLANNED/DESIGN ONLY**  
Corte de código `moragaga/atlanticus@94f26213ca28b550baf53d8ee34e34da7538ad17`; canonical base `ed1edd7a04533cf32a2b32844cb79b9edbe7bcc8`; decisions consultadas parcialmente `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`. La antigua caracterización «Master sin código» es **SUPERSEDED** para 001A/B/C, no para la ejecución Apply.

## Problema y frontera vigente

Cuando el ambiente comienza sin Profiles, ADA Access ni usuarios promovidos, el Manager ordinario puede carecer de los datos necesarios para autorizar al operador. Su página interna **Proyección de usuarios** y el servicio de recuperación ya implementado usan la identidad y permisos del Manager; no constituyen por sí solos el bootstrap vacío. Master Projection es una **página independiente**, alojada en la composición Web existente y accesible por URL propia, con credenciales de servicio externas al Manager. No concede permisos del Manager, no edita/publica Sources y no requiere un nuevo proceso remoto.

## 001A — material protegido implementado

Archivos de autoridad:

```text
tooling/distribution/web/starter/ada/tooling/master_projection.py
tooling/distribution/web/starter/ada/src/application/master_projection/material.py
tooling/distribution/web/starter/ada/src/application/master_projection/reader.py
```

- El tooling ADA pide interactivamente usuario de servicio y contraseña; crea fuera del proyecto distribuido un ZIP **AES-256** con un miembro `master-projection.json`, verificador de contraseña **scrypt** y etiquetas `material_id`, `application_namespace`, `environment` y acciones declaradas.
- Acciones declaradas por el material: `projection.preview`, `projection.apply`, `users.replace`. **001C solo implementa `projection.preview`**, y la existencia de una acción en el ZIP no autoriza ninguna ruta de escritura futura por sí sola.
- El lector inspecciona `ABSENT`, `PRESENT` o `INVALID`, verifica el ámbito y genera fingerprint SHA-256 para invalidación al sustituir el archivo. La ruta configurada es **absoluta y externa** cuando se usa.
- El material no se consume con el primer acceso ni se revoca solo por proyectar. Ante pérdida de contraseña se genera y distribuye nuevo material; no guardar contraseña en `.env`, repositorio, wheelhouse ni Starter.

## 001B — planner read-only implementado

`compose_master_projection_planner(stores: ConfigurationManagerStores)` construye el planner sobre Stores y proyectores preexistentes. Inspecciona los pares **Navigation, Profiles, Tools, ADA Access, KPI Registry y KPI Definitions**; obtiene dependencias semánticas del contrato real y preserva targets completos. Distingue `SOURCE_MISSING`, `CURRENT`, `NEVER_PROJECTED`, `OUTDATED`, `BLOCKED`, `UNAVAILABLE`. Users se consulta **por separado** mediante catálogo de snapshots aprobados y nunca es automáticamente ejecutable.

Las dependencias del wiring actual son ADA Access → Profiles; KPI Registry → Tools; KPI Definitions → KPI Registry. No inventar orden global para dominios independientes.

## 001C — página protegida HTTP implementada

Rutas actuales exactas:

```text
GET/POST /master-projection
POST     /master-projection/logout
```

La composición registra el middleware Master **antes** de Identity/Navigation para atender sus dos rutas independientes sin exigir un `AccessSnapshot` de Manager. Cualquier otra ruta, incluida `/master-projection/unknown`, sigue el control de Identity ordinario; prohibido un bypass por prefijo.

**Material AUSENTE:** respuesta informativa controlada; no formulario ni acciones privilegiadas. **Material INVÁLIDO:** error informativo controlado, sin autenticar; código 503. **Material PRESENTE:** formulario propio de usuario/contraseña con CSRF. El plan solo se consulta después de desbloquear correctamente el material para namespace y ambiente actuales y confirmar `projection.preview`.

La sesión Master utiliza clave propia en Flask, vence a los **900 segundos** desde emisión (no se renueva automáticamente por navegar), invalida el acceso si desaparece/cambia el archivo y protege logout con `POST + CSRF`. Las respuestas Master usan `Cache-Control: no-store, private` y otras cabeceras de seguridad. La página autenticada es **solo lectura**: lista el estado de las seis proyecciones y Users; no presenta botones de Apply/REPLACE. El hecho de compartir el host y el mecanismo de cookie Flask no fusiona los roles Manager y Master.

`ADA_MASTER_PROJECTION_MATERIAL_PATH` es una variable opcional de configuración de ADA Generic/Starter. El Starter usa `StarterMasterMaterialReader` y `create_worker_runtime()`. **La ruta externa demostrada no constituye un uploader/warmup automático calificado**.

## Qualification delimitada

**VERIFIED STATIC:** existen los commits `73603ee...`, `74f9107...`, `e217754...`, `94f2621...` con los archivos correspondientes. **VERIFIED USER-REPORTED:** 38/38 tests seleccionados previos, 39/39 regresiones previas, 3/3 integración HTTP corregida, 6/6 Reader+Runtime, 2/2 settings post-correctivo. Estos conjuntos se solapan y se ejecutaron en momentos distintos. En una distribución local v2 se obtuvieron 67 wheels `BUILT_UNQUALIFIED`, `SYNCED`, HTTP 200 con/sin material y login real positivo. La distribución fue generada desde cambios locales antes de publicar `e217754`; su `source_git_head` anunciado `efe231d...` no representa el árbol no commiteado por sí solo.

**UNVERIFIED:** rerun conjunto después del correctivo final, validación manual expresa de las seis entradas/estados en navegador, logout/relogin manual, qualification de artifact desde checkout limpio del HEAD final, autorización productiva, múltiples workers reales y Azure/Entra.

## Decisiones congeladas y tensiones OPEN

- `Master != Manager`; Identity normal no da permisos Manager automáticamente. Las excepciones Master no se extienden a rutas desconocidas, otras páginas ni callbacks.
- El formato de 001A puede declarar acciones futuras, pero aplicar proyecciones y reemplazar Users siguen fuera del código 001C.
- Reutilizar `ConfigurationManagerStores` y servicios presentes, sin duplicar coordinador ni inventar modelos Legacy.
- Usuarios se recuperan solo desde snapshot **expresamente aprobado**. El provider durable del Manager deriva `identity_realm` desde promovidos y no justifica destinos sin promovidos.
- **OPEN / CONTRACT CONFLICT:** cannonical histórica señala una baseline Entra para todas las superficies pre-Manager; Master implementa una excepción independiente de servicio. Exige revisión específica de decisiones históricas pertinentes y acuerdo formal de límites/controles antes de habilitar producción. Los DOCX no fueron revalidados exhaustivamente en este cierre.
- **OPEN / OPERATIONS:** ubicación/aprovisionamiento productivo del ZIP, revocación/rotación operacional, trazabilidad y posibles fallos parciales deben tratarse con componentes existentes y evidencia real.

## Único incremento siguiente

`MASTER-PROJECTION-001D` **PLANNED / SOLO CONTRATO**: inventariar proyectores/Stores existentes y diseñar la autorización/revalidación/confirmación/auditoría de `projection.apply` para las seis proyecciones ordinarias, sin código hasta consenso. `users.replace` es un frente/gate independiente, igual que warmup, Entra, vínculo organizacional ADA, Home, collectors y alarmas. No reabrir 001A/B/C por estética o refactor sin finding concreto.
