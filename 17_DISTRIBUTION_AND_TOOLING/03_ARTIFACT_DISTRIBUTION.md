# Artifact and Distribution Boundary

Estado: **CURRENT / WEB PORTABLE HISTÓRICO CLOSED / DOCKER PARTIAL / PRODUCTIVE PIPELINE OPEN**

Inspección de frontera actual para el traspaso: `moragaga/atlanticus@392ee281a32396516fb08c23c63514d8cbdb3489`. Qualification Web Starter histórica sobre checkpoint previo `c2bf25e...`; no atribuirle rebuild ni tests en HEAD actual.

## Frontera contractual

```text
SOURCE → ARTIFACT → DISTRIBUTION INPUT
```

Atlanticus produce artifact y contrato de entrega. El pipeline corporativo y el deployment específico pertenecen a DevOps/consumidor; no crear un framework paralelo. Los wheels internos y externos del wheelhouse son archivos separados, no librerías públicas embebidas dentro de nuestros wheels. La necesidad de cada rueda externa debe auditarse contra locks y dependencias antes de eliminarla.

## Backend — implementación existente, sin requalification en este cierre

Hay scripts de generación y validación de procesos y `deployment/local/generate_compose.py` para artifacts **de procesos**. El generador actual descubre artifacts, valida `.env` locales, contratos de proceso y genera un workspace Compose específico. No atribuirle perfiles `infra/app/full` ni una orquestación integrada ADA Web + Cosmos + Azurite sin evidencia. Revisar `deployment/processes`, scripts y contracts vigentes en el nuevo chat; no recuperar referencias históricas obsoletas como `scripts/local-process.sh` sin comprobar su existencia.

## Web — CURRENT

- `tooling/distribution/web/generate_starter.py` crea Starters editables Generic/ADA con manifest.
- `build_wheelhouse.py` construye la clausura runtime+build offline a partir de locks, ruedas internas/externas, hashes SHA256 y tags compatibles.
- `qualify_starter.py` prueba instalación aislada y endpoints, callbacks y assets.
- `distribution/` es salida generada, no una nueva fuente de código.

**VERIFIED MANUAL HISTÓRICO:** SOURCE_SMOKE y PORTABLE para Generic/ADA bajo Python 3.14.2; wheelhouses observados Generic 36 y ADA 108. No es una prueba de generación desde el HEAD actual ni una aprobación para eliminar ruedas externas indiscriminadamente.

## Docker actual y dirección posterior

En el código inspeccionado, el template Web es multistage offline con `python:3.14.2-slim-bookworm`, verificación de wheelhouse, usuario no root, healthcheck, `python -m application`, puerto interno **8050** y restricción **local-only**. Las pruebas históricas verificaron build y `/health/live` de Generic/ADA, pero no readiness integral ni producción. ADA `/example` devolvió `Acceso denegado` con Manager `disabled`.

**PROPOSED / NOT IMPLEMENTED AS VERIFIED HERE:** un Dockerfile Web único reutilizable para local/producción, host Gunicorn y puerto interno `8000`, configuraciones/secretos inyectados y bootstrap real sin identidad local ficticia en producción. La baseline objetivo global es Python `3.14.7` e imagen `python:3.14.7-slim-trixie`; los paquetes Web/Starter observados aún conservan metadata y template `3.14.2`. La migración requiere inventario y decisión explícita, no un reemplazo ciego.

## Variables, pipeline y servicios — fronteras OPEN

`*.env.detail` son contratos documentales **sin secretos**. Las plantillas inactivas y mappings para DEV/UAT/PRD son intención anterior, no archivos efectivos verificados durante este cierre; auditar los templates que realmente existan antes de definir nombres o valores. Valores derivados deben identificarse a partir del código, nunca suponerse. DevOps/host resolverá secretos conforme al pipeline real.

Cosmos/Azurite local y propuesta `infra`, `app`, `full`: **PLANNED / UNVERIFIED COMO COMPOSE INTEGRADO**. Existen herramientas locales separadas en `connectivity/docker/` y Compose de procesos; comprobar qué es reusable antes de acordar una única topología. En producción la infraestructura base se prepara externamente; la Web puede asegurar/validar sólo recursos de aplicación autorizados. Separar Tool Source, Tool Projection y KPI Delivery cuando el contrato lo exige.

## Gates no equivalentes

| Frontera | Estado |
|---|---|
| SOURCE_SMOKE y PORTABLE Web históricos | CLOSED / VERIFIED MANUAL |
| Docker Web build y liveness históricos | VERIFIED MANUAL / PARTIAL |
| Header/colores en ADA Generic core local | CLOSED / VERIFIED MANUAL, alcance distinto |
| Starter ADA Manager + Navigation HTML | OPEN / UNVERIFIED |
| Rebuild de artifacts y wheelhouses desde HEAD nuevo | OPEN / UNVERIFIED |
| Unificación Docker / Gunicorn / puerto 8000 | PLANNED / UNVERIFIED |
| env/pipeline/secret mapping; Compose local integrado | PLANNED / UNVERIFIED |
| Entra, Cosmos/Azurite restart real, CI y Azure | PLANNED / UNVERIFIED |

**NEXT único:** auditoría de artifacts/locks/generadores sobre HEAD vigente. La decisión de implementación se tomará después de esa auditoría; no abrir Docker y Compose en el mismo incremento.
