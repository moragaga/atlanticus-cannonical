# Web Platform — Deployment Order

Estado: **CURRENT DIRECTION / ADA GENERIC PARTIALLY IMPLEMENTED / E2E OPEN**

Checkpoint de implementación del cierre parcial: `moragaga/atlanticus@ce1213ec14cdee0be905c042c1cf513d71fb5b2d`.

## Orden objetivo productivo

```text
0. Base Cloud Infrastructure
   ├── App Service / Container App
   ├── Cosmos account + database
   ├── Storage account
   ├── identity / secrets / networking
   └── demás infraestructura administrada
             ↓
1. WEB
             ↓
2. RESOURCE PREPARATION
   ├── validate configuration
   ├── ensure/validate Cosmos containers
   ├── ensure/validate Storage resources cuando exista capability
   └── register external backend requirements
             ↓
3. SOURCE / PROJECTION PREPARATION
   ├── discover saved releases/files
   ├── compute projection plan
   └── project required configuration
             ↓
4. WEB READY / MANAGER READY
             ↓
5. BACKEND
   ├── producers
   ├── KPI jobs
   ├── Alarm Runtime
   └── demás jobs
```

**Web se despliega antes que Backend.** Eso no exige datos de negocio al iniciar la Web: Web debe ofrecer preparación, diagnóstico y evidencia de readiness antes de habilitar productores.

## Local Docker — objetivo

```text
Docker infra/emulators
      ↓
Web
      ↓
create local DB if missing
      ↓
ensure containers/resources
      ↓
project saved configuration
      ↓
Backend jobs
```

El objetivo `docker compose up` sobre un entorno limpio **no está cerrado**.

## Qué existe en ADA Generic — CURRENT

`ADA_MANAGER_PERSISTENCE_PROVIDER=durable` permite a la composición local conectar Manager con Blob Source, Cosmos Projection y Users Registry/Runtime; el CLI de recursos se ejecuta por separado:

```text
uv run ada-generic-manager-resources ensure-local
uv run ada-generic-manager-resources validate
uv run ada-generic-application
```

`ensure-local` comprueba que exista el contenedor Blob configurado y luego asegura la base Cosmos y los seis contenedores **del plan Manager**. `validate` no modifica recursos. El contenedor Blob aún debe prepararse fuera de ese comando. Ni el plan global de aplicación ni la automatización completa de Docker están implementados aquí.

## Cloud — dirección, sin declarar completado

```text
Support / IaC
→ infraestructura base (incluida base Cosmos y cuenta Storage)
→ Web
→ recursos de aplicación permitidos
→ projection bootstrap
→ Backend
```

No ejecutar `ensure-local` en `production`. La identidad productiva concreta y los permisos Cloud de aprovisionamiento no se cualificaron en este hito.

## Criterio de soporte futuro

1. Preparar infraestructura base.
2. Desplegar Web.
3. Verificar bootstrap/readiness cuando exista superficie integrada.
4. Ejecutar/confirmar proyecciones.
5. Confirmar READY.
6. Desplegar/habilitar Backend.

Esta secuencia es un contrato de dirección. La prueba siguiente se limita a reproducir y verificar el flujo ADA Generic/Manager durable, sin declarar cerrados los productores u otras aplicaciones.
