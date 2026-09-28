# Web Platform — Deployment Order

Estado: **CURRENT DIRECTION / ADA GENERIC RESOURCE PREPARATION LOCAL VALIDATED / E2E OPEN**  
Implementación: `moragaga/atlanticus@da75752e87036b8318f38f8d405c55e8cb18717d`; qualification Docker local reportada el 2026-09-28.

## Secuencia objetivo productiva — contrato, no calificación Cloud

```text
0. Base Cloud Infrastructure
   ├── App Service / Container App
   ├── Cosmos account + database
   ├── Storage account
   ├── identity / secrets / networking
   └── otras dependencias de host
             ↓
1. WEB (proceso y capacidades base)
             ↓
2. RESOURCE PREPARATION
   ├── validar configuración de providers
   ├── asegurar/validar contenedores Cosmos autorizados
   ├── dejar Blob productivo completamente fuera de este preflight
   └── diagnosticar parcialmente fallos por recurso
             ↓
3. SOURCE / PROJECTION PREPARATION
   ├── descubrir releases/files existentes
   ├── planificar desde dependencias reales
   └── proyectar configuración pertinente
             ↓
4. APPLICATION / MANAGER OPERATIONAL READINESS
             ↓
5. BACKEND
   ├── productores
   ├── KPI jobs
   ├── Alarm Runtime
   └── otros jobs
```

La Web se despliega antes que Backend, pero la existencia del proceso no acredita permisos administrativos ni datos listos. Este orden de fases no establece un orden global artificial entre `ProjectionTarget` independientes.

## ADA local — CURRENT

```text
Docker Cosmos Emulator + Azurite
          ↓
Web + resource job como procesos independientes
          ↓
Resource Preparation: Blob, base Cosmos, seis contenedores
          ↓
Validación explícita / posterior proyección
          ↓
Backend, cuando cada dominio cumpla sus precondiciones
```

En `tooling/distribution/web/starter/ada/deployment/compose/full.yaml`, `web` **no depende del éxito** de `resources`. Ambos dependen sólo del inicio de `cosmos-emulator` y `azurite`. El job `resources` espera `/ready` de Cosmos y socket Azurite antes de preparar. No afirmar que `web` está inmediatamente funcional ni que una inicialización de Tool puede completarse con Cosmos detenido.

CLI y job actuales:

```text
ada-generic-manager-resources prepare
ada-generic-manager-resources validate
python -m application.local_resources    # job exclusivo Compose local
```

`ensure-local` fue reemplazado en el entrypoint actual por `prepare`; no conservarlo como instrucción operacional vigente. `validate` no crea recursos. Local `prepare` sí crea el contenedor Blob faltante; no requiere creación manual previa. El conjunto está **limitado al plan ADA Manager actual**: no crea de manera implícita los recursos de KPI Delivery, Command Center o alarmas.

## Qualification del flujo parcial — VERIFIED USER-REPORTED

Se generó distribución ADA con 67 wheels internos y `PRECHECK_PASS`, se construyó la imagen `ada-generic:resource-validation-001`, se desplegó con contenedores/volúmenes nuevos y el job inicial generó ocho `CREATED`/salida 0; validación posterior ocho `READY`. Se comprobó idempotencia, reinicio de emuladores preservando topología, error parcial Cosmos con Blob intacto y recuperación de validación sin reiniciar Web. `/health/live` respondió 200 durante inicialización de recursos y cuando Cosmos cayó **después** de arrancar Web.

No se ha probado aún publicación/proyección de Sources, inicialización Master, usuarios finales ni circulación de KPI a través de este Starter completo. El arranque frío con Cosmos caído no cumplió la ventana de respuesta de diez segundos; véase `04_WEB_READINESS_AND_DECOUPLING.md`.

## Cloud — PLANNED / UNVERIFIED

La base Cosmos y cuenta Blob deben existir externamente; `prepare` productivo sólo asegura contenedores Cosmos faltantes y **omite toda operación Blob**. Permisos reales, identidad Entra del Manager, observabilidad remota y ejecución Azure siguen sin probarse. No atribuir calificación productiva por haber probado emuladores locales.

## Siguiente frontera

**MASTER-PROJECTION-001** (ver `06_PRE_MANAGER_BOOTSTRAP_SURFACE.md`): página aislada para la proyección inicial cuando falta la autorización ordinaria, material protegido generado por tooling ADA existente y sus dos estados obligatorios de acceso. Primero inspección y contrato; luego implementación únicamente tras autorización. No implementar en este hito la asignación organizacional ADA, Home degradado ni componentes dinámicos.
