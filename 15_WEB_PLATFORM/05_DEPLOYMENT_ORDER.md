# Web Platform — Deployment Order

Estado: **CURRENT DIRECTION**

## Orden real productivo

La secuencia de aplicación queda:

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

## Regla

**Web se despliega antes que Backend**.

Esto no significa que Web deba tener datos para arrancar.

Significa que Web es el punto de preparación y diagnóstico de recursos/configuración antes de habilitar procesos productores.

## Local Docker

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

El objetivo es:

```text
docker compose up
```

sobre un ambiente limpio sin crear manualmente la base funcional.

## Cloud

```text
Support / IaC
→ base resources

Web
→ application resources
→ projection bootstrap

Backend
→ processing
```

## Soporte

Este flujo permite entregar a soporte una secuencia clara:

1. preparar infraestructura base;
2. desplegar Web;
3. verificar página de bootstrap/readiness;
4. ejecutar/confirmar proyecciones;
5. confirmar READY;
6. desplegar/habilitar Backend.

La Web actúa como evidencia de readiness del producto.
