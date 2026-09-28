# Web Platform — Open Items

Estado: **MASTER 001A/B IMPLEMENTED/CLOSED; 001C IMPLEMENTED / FINAL QUALIFICATION IN PROGRESS; 001D PLANNED / DESIGN-ONLY**  
Corte específico Master contrastado: `moragaga/atlanticus@94f26213ca28b550baf53d8ee34e34da7538ad17`. Resto de cierres se conserva según sus propios checkpoints; no recalificar otros frentes desde esta página.

## CLOSED / CURRENT en sus alcances acreditados

- **Users Recovery backend y Users Projection dentro del Manager:** snapshots aprobados, RESTORE estricto y REPLACE con before-image/auditoría; UI de dos pestañas y modal. Calificación anterior en laboratorio, **no** acreditación integral de seguridad/producción; consultar `13_USERS_PROJECTION_RECOVERY.md`.
- **Resource Preparation 001 ADA Generic:** código registrado en `da75752e87036b8318f38f8d405c55e8cb18717d`. En Docker local se informaron ocho recursos creados, idempotencia, reinicio conservando topología, fallo Cosmos parcial y recuperación. No acredita cold start Web con Cosmos caído ni publicación real; consultar `03_RESOURCE_PROVISIONING.md`.
- **Master 001A:** formato/generador/lector de material protegido en el tooling ADA del commit `73603ee...`, con verificaciones criptográficas y ámbito app/environment.
- **Master 001B:** planner read-only de seis pares y Users separado en el commit `74f9107...`.
- **Master 001C código:** commit `e217754...` con 19 archivos y corrección `94f2621...` en un test. La implementación existe y la página HTTP local funciona; la qualification formal completa sigue abierta.
- **Starter, Navigation y Manager históricos:** conservar su evidencia versionada, sin promover sus tests antiguos a una CI general sobre este HEAD.

## OPEN / razón / frontera de aceptación

| Ítem | Estado | Razón y alcance pendiente |
|---|---|---|
| MASTER-001C rerun conjunto tras `94f2621` | **OPEN / UNVERIFIED** | 38/38 tests 001C y 39/39 regresiones anteriores; prueba conjunta posterior al fix de integración falló en settings por variable exportada, corregida después. El test corregido tuvo 2/2 PASS con variable presente; no hay resultado de toda la suite **posterior** al último HEAD. |
| MASTER-001C vista de seis proyecciones tras login real | **OPEN / UNVERIFIED MANUAL** | El usuario confirmó que pudo ingresar al material real y que GET devolvió 200; no aportó confirmación explícita de los seis estados mostrados. |
| MASTER-001C logout y reautenticación real | **OPEN / UNVERIFIED MANUAL** | Tests unitarios verifican POST/CSRF/invalidez de sesión, pero falta ensayo explícito del operador en el artifact. |
| Master distribución desde HEAD limpio calificado | **OPEN / UNVERIFIED** | Smoke v2 con 67 wheels `BUILT_UNQUALIFIED`, init `SYNCED` y HTTP 200 con/sin ZIP real. `source_git_head: efe231d...` corresponde al commit anterior a publicar el working tree de 001C; no hay qualify final del `94f2621`. |
| MASTER-001D `projection.apply` | **PLANNED / DESIGN GATE** | Inspeccionar proyectores existentes y decidir autorización por operación, revalidación de target, confirmación, idempotencia, resultados, fallos parciales y auditoría **sin inventar código**. |
| Material Master: warmup/upload/custodia productiva | **OPEN / UNVERIFIED** | Generador y lector externos existen; solo está probado `ADA_MASTER_PROJECTION_MATERIAL_PATH`, no un pipeline automático de subida/warmup ni Azure. |
| Master excepción pre-Manager frente a Entra | **OPEN / CONTRACT CONFLICT** | La excepción técnica con credencial independiente está implementada; delimitarla formalmente respecto de baseline Entra histórica antes de habilitar producción. No consta lectura exhaustiva de los DOCX de decisiones. |
| Users Master en destino sin promovidos | **OPEN / BLOCKED PARA REPLACE** | `identity_realm` del provider del Manager deriva de promovidos; definir origen aprobado y compatibilidad `issuer/subject_id`, condiciones de mantenimiento y autorización antes de escribir. |
| Users REPLACE: fallos parciales y revocación | **OPEN / SECURITY GATE** | Faltan interrupción real, retries, aislamiento/concurrencia y revocación efectiva, sin equivaler el check de UI a un control operacional. |
| Users RESTORE/REPLACE invasivo desde navegador | **UNVERIFIED** | Flujo de laboratorio y validación visual de la UI del Manager no certifican escritura destructiva desde navegador. |
| Transporte interambientes de snapshot aprobado | **OPEN / OTHER GATE** | Compatibilidad de issuer, registro y proyección destinatarios debe verificarse expresamente. |
| `APPLICATION-RESOURCE-PLAN` global | **PLANNED / OTHER FOCUS** | Resource Preparation 001 cubre solo los recursos acordados de ADA Manager; no todo Atlanticus/ADA. |
| Infraestructura Azure productiva / Entra / telemetría | **UNVERIFIED / OTHER FOCUS** | No consta ensayo en infraestructura productiva ni validación de permisos/secretos/observability. |
| Documentos de negocio reales y pipeline proyección E2E | **UNVERIFIED / OTHER FOCUS** | Persistir topología emulada no prueba Source/Projection reales ni su publicación. |
| Arranque frío Web con Cosmos caído | **OPEN / DEFERRED** | Se observó timeout `/health/live` a 10 s; tras restaurar Cosmos la Web ya arrancada sí respondió. No declararlo cerrado. |
| Home predefinido con Cosmos vacío/caído | **PLANNED / LATER FOCUS** | Estructura visible, estados ausente vs no disponible y carga Tool aún no certificados visualmente. |
| Reanudación del KPI Collector tras corte Cosmos con browser real | **UNVERIFIED E2E / LATER FOCUS** | Hay código y tests unitarios de reintento/cache, no evidencia de entrega real al Home con Tool/Docker. |
| Estados futuros de componentes dinámicos | **PLANNED / LATER FOCUS** | Evaluar por componente cuando se implemente; no atribuir errores PI/Dispatch a KPI Delivery sin evidencia. |
| Identificación organizacional ADA de usuarios | **PLANNED / OTHER FOCUS** | Cargo, área y grupo requieren su propio Source/Projection/ownership; no incorporarlos al Core Users ni a Master. |
| Bloqueo detallado de proyección cuando falla Cosmos/Blob | **PLANNED / OTHER FOCUS** | Política operacional/UI diferida; MASTER-001D puede estudiar solo lo estrictamente necesario para las seis proyecciones. |
| Python objetivo Project `3.14.7` vs Web/Starter `3.14.2` | **OPEN / SEPARATE** | No migrar dependencias, imágenes ni metadatos durante Master. |
| CI monorepo, Ruff global y seguridad multiworker productiva | **UNVERIFIED / SEPARATE** | Pruebas seleccionadas no son certificación integral. |

La hipótesis histórica de un extra Users/Cosmos permanece diferida, sin permiso para cambiar packaging. Los otros frentes (Alarm Engine y ADA Command Center) no se reabren con este documento.

## Orden de continuidad sin mezclar incrementos

1. **NEXT ÚNICO:** MASTER-PROJECTION-001D, **solo inspección y debate** de `projection.apply` para los **seis dominios ordinarios**. Al inicio, registrar los tres gates de qualification 001C pendientes. Ninguna implementación antes del consenso/autorización.
2. `users.replace` desde Master y seguridad del destino vacío se discuten en un incremento/gate separado, no se introducen automáticamente en 001D.
3. La integración material/warmup productiva y compatibilidad excepcional con baseline Entra requieren acuerdos formales propios.
4. Los focos diferidos de organización ADA, Home y recuperación del collector, nuevos componentes, Python y Azure siguen independientes.

No reabrir 001A/B/C ya implementados por razones estéticas ni crear adaptadores legacy para el siguiente incremento.
