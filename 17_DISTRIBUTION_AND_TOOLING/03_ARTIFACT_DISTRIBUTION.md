# Artifact and Distribution Boundary

Estado: **CURRENT / WEB PORTABLE QUALIFIED / DOCKER PARTIAL / PRODUCTION OPEN**

Checkpoint Web inspeccionado: `moragaga/atlanticus@c2bf25e353b890dc8fd8553ad375745d23ec7154`.

## Frontera contractual

```text
SOURCE → ARTIFACT → DISTRIBUTION INPUT
```

Atlanticus produce artifact y contrato de entrega. El pipeline corporativo y el despliegue concreto pertenecen a DevOps/consumidor; no crear un framework de distribución paralelo.

## Backend — contexto histórico no revalidado aquí

`deployment/processes/bundle.py` y scripts existentes proporcionan generación y controles backend. Los procesos KPI tienen su propio alcance. Este cierre Web **no** reejecutó builds de procesos backend ni comprueba que la referencia histórica `scripts/local-process.sh` exista hoy.

## Web — CURRENT

`generate_starter.py` produce Starters editables Generic/ADA con manifest por archivo. `build_wheelhouse.py` construye la clausura de dependencias runtime y build a partir de locks, incorporando ruedas internas y externas con integridad SHA256 y compatibilidad de plataforma. `qualify_starter.py` prueba instalación aislada offline y comportamiento de endpoints/callback/assets.

Evidencia manual compartida: `SOURCE_SMOKE / PASS` y `PORTABLE / PASS` para ambos perfiles con Python 3.14.2; wheelhouses observados de **36** y **108** archivos, respectivamente. Los wheels de bibliotecas externas son dependencias directas/transitivas o de build; no están embebidos dentro de nuestros wheels, ni todas las ruedas del wheelhouse necesariamente se instalan en runtime final.

## Docker parcial

Template actual multistage: `python:3.14.2-slim-bookworm`, instalación offline, verificador de hashes, usuario no root, healthcheck, `python -m application`, puerto **8050**, entorno **local-only**. El usuario construyó ambas imágenes; ambas contestaron `/health/live`. Generic sirvió `/example`; ADA devolvió una página HTML de acceso denegado. No interpretar liveness como autorización de Navigation, Manager accesible o despliegue productivo.

## Estados de qualification independientes

| Frontera | Estado |
|---|---|
| SOURCE_SMOKE Generic y ADA | CLOSED / VERIFIED MANUAL |
| PORTABLE Generic y ADA | CLOSED / VERIFIED MANUAL |
| Docker build/liveness de ambas imágenes | VERIFIED MANUAL / PARTIAL |
| ADA `/example` permitido vía Navigation en navegador | OPEN / FINDING |
| Manager Home/header/sidebar/Navigation visibles desde Starter | OPEN |
| Host Gunicorn y puerto 8000 local+producción | PROPOSED / PLANNED |
| Identidad productiva y despliegue Azure | PLANNED / UNVERIFIED |
| Cosmos/Azurite local, restart y aprovisionamiento | PLANNED / UNVERIFIED |

## Decisión posterior, todavía no implementada

La opción recomendada es **un solo Dockerfile** para local y producción, con Gunicorn, puerto interno **8000**, healthcheck y configuración/secretos inyectados. El punto de entrada productivo debe reutilizar la composición real y no inventar una identidad local en producción. Esta recomendación no sustituye al Dockerfile CURRENT hasta que se implemente y cualifique.

## Configuración y servicios

Los templates de secretos y mappings DEV/UAT/PRD viajarán inactivos y sin valores sensibles; su incorporación permanece PLANNED. Los servicios locales Cosmos/Azurite apoyarán qualification, pero el artifact no crea infraestructura Cloud productiva. Los contratos Tool Source, Tool Projection y KPI Delivery pueden requerir conexiones distintas.

Las referencias canónicas previas que describían toda la Web portable como UNVERIFIED están **SUPERSEDED para la prueba PORTABLE observada**, no para producción, navegación real ni Azure. Python actual 3.14.2; 3.14.7 es migración futura.
