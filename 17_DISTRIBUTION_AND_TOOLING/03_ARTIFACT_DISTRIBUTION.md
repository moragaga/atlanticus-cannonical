# Artifact and Distribution Boundary

Estado: **CURRENT / WEB STARTER + COMPOSE FULL LOCAL VERIFIED / PRODUCTIVE PIPELINE OPEN**

Inspección estática del cierre: `moragaga/atlanticus@ebc7a8bf8d49e931fd4e2487dac5ee036011a0a5`; Canonical base para reemplazo: `moragaga/atlanticus-cannonical@7d0de8d9fa27f28170171588bc219c2bae99f34e`. Qualification ejecutada por el usuario en checkpoints anteriores. No confundir ejecución parcial con una qualification global del HEAD.

## Frontera contractual

```text
SOURCE → ARTIFACT → DISTRIBUTION INPUT
```

Atlanticus produce artifacts y contrato de entrega. Pipeline corporativo, infraestructura productiva y despliegue específico pertenecen a DevOps/host. Los wheels internos y externos del wheelhouse son archivos separados; no concluir que se pueden eliminar dependencias externas sin auditar locks y closure.

## Backend y Web — ownership independiente

Los scripts backend y `deployment/local/generate_compose.py` producen workspaces de artifacts **de procesos**, no son los perfiles Compose del Starter Web. No mezclar ambos workflows ni introducir un orquestador nuevo por semejanza de nombres.

En Web, `tooling/distribution/web/generate_starter.py` crea Starters editables Generic/ADA con manifest; `build_wheelhouse.py` arma closure offline y `qualify_starter.py` comprueba instalación y capacidades declaradas. `distribution/` es salida generada, no código fuente nuevo.

Qualification histórica reportada: Generic+ADA SOURCE_SMOKE y PORTABLE bajo Python 3.14.2, wheelhouses Generic 36/ADA 108 en checkpoint anterior. No atribuir esas cifras al nuevo artifact ni al HEAD de esta revisión.

## Incrementos recientes ADA Starter / Docker Compose

CURRENT en `tooling/distribution/web/starter/ada/`:

- Starter editable, distribución/wheelhouse y servidor Gunicorn con lifecycle por worker (previamente corregido).
- `deployment/compose/infra.yaml`, `web.yaml`, `full.yaml` y `tooling/project.py` con comandos `compose {build,up,down,logs,ps,prepare}` y perfiles `infra|web|full`.
- `full`: Cosmos emulador vNext, Azurite Blob, `resources` inicializador, Web y red/volúmenes externos. `web` depende de recursos preparados y consume Source Blob/Projection Cosmos; se respetan sus namespaces y rutas lógicas.
- `ADA_MANAGER_PERSISTENCE_PROVIDER=durable`; `ADA_TOOL_SOURCE_PROVIDER=blob`, `ADA_TOOL_PROJECTION_PROVIDER=cosmos` en `full`. Manager CURRENT tiene modalidades `local` y `durable`; Tool permite combinaciones independientes según su propio contrato. No prometer que Manager soporta todas las combinaciones de Tool.

**VERIFIED AUTOMATED REPORTADO:** `33 passed in 0.70s` en pruebas específicas `test_project_tool.py` y `test_compose_integration.py` en el equipo con Docker. 

**VERIFIED MANUAL REPORTADO:** generación de distribución, precheck `PRECHECK_PASS`, build de imagen, inicio de Cosmos/Azurite, job `resources` finalizado con seis contenedores y Gunicorn con tres workers. La consola inicial de `ps full` mostraba health en fase `starting`; no usarla como prueba de readiness final. El usuario confirmó funcionamiento. No se dispone aquí de prueba explícita de persistencia de configuraciones después de `down/up` sin reproyección.

**VERIFIED TESTS del patch visual posterior:** el usuario aplicó el patch de etiquetas del Manager sin errores y ejecutó dos invocaciones de pruebas (once puntos exitosos y `6 passed`); falta comprobación visual en artifact reconstruido.

## Producto futuro, independiente del Manager

Una página aislada de proyección (PLANNED) permitirá inicializar un ambiente cuando no hay perfiles/usuarios promovidos en Cosmos. Consumirá las configuraciones ya disponibles en Storage y las proyectará con APIs de dominio existentes, más el mecanismo especial de Users que se debe crear primero. No es un nuevo Manager remoto ni un editor de módulos. Credenciales de proyecto solicitadas durante la operación; no vencen automáticamente ni consumen el paquete, permiten reintentos y regeneración manual cuando se pierden. Exacta implementación de tooling, protección criptográfica y autorización pre-Users: **OPEN / NO CODE**.

No introducir este diseño en generadores actuales hasta congelar contrato y autorizaciones. Source de Users actual es registro de **aplicación**, no de cada herramienta; la asignación operacional futura podrá ser de herramienta, después de su propio diseño.

## Estado de env/pipeline

`*.env.detail` son contratos documentales sin secretos. El uso de mappings para DEV/UAT/PRD y la resolución productiva de secretos aún requieren un gate propio; no inventar valores ni poblar secrets del repositorio. Producción usa identidad y recursos externos legítimos: el archivo `tooling/distribution/web/starter/ada/src/application/production.py` todavía exige inyección de `IdentityProvider` por el host.

Python objetivo Project `3.14.7` e imagen `python:3.14.7-slim-trixie`; en la implementación Web inspeccionada se observan todavía `3.14.2` y `python:3.14.2-slim-bookworm`. Es un desfase real, no rebasarlo silenciosamente en este cierre.

## Gates separados

| Frontera | Estado / evidencia |
|---|---|
| SOURCE_SMOKE / PORTABLE Web anteriores | CLOSED / VERIFIED MANUAL HISTÓRICO |
| ADA Compose `full` generación, build, arranque emuladores + web | CLOSED / VERIFIED MANUAL; tests Compose 33 |
| Provider labels patch | TESTS VERIFIED REPORTADOS; visual artifact UNVERIFIED |
| Persistencia ADA de datos tras reinicio + rutas HTML completas | UNVERIFIED sin evidencia terminal concluyente específica |
| Entra productiva, Azure deployment, mapping, CI general | PLANNED / UNVERIFIED |
| Página aislada de proyección/credenciales | PLANNED / NO IMPLEMENTACIÓN |
| Users validación/recovery desde registry aprobado | PLANNED / NEXT, scope diferente |

No reabrir tooling de distribución en `USERS-PROJECTION-RECOVERY-001`. El nuevo proceso deberá primero establecer su contrato backend y luego ser consumido por la página externa.
