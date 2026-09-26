# Frontend Generation

Estado: **CURRENT / SOURCE_SMOKE + PORTABLE CLOSED / DISTRIBUTION INTEGRATION OPEN**

Inspección de implementación: `moragaga/atlanticus@c2bf25e353b890dc8fd8553ad375745d23ec7154`.
Resultados de ejecución proporcionados por el usuario el 2026-09-26; no son tests ejecutados por el asistente sobre su equipo.

## Objetivo y frontera

Atlanticus genera un Starter Web **editable y reusable**; no un nuevo framework ni una Tool ADA definitiva. La base `generic` no depende de ADA. El overlay `ada` reutiliza `ada-generic-application`, su bootstrap operacional y su composición externa. El consumidor externo implementa la aplicación concreta.

```text
WebModule / page_packages / AssetLayer / register_callbacks
             ↓
tooling/distribution/web/generate_starter.py
             ├── generic → distribution/generic-web-starter
             └── ada     → distribution/ada-web-starter
             ↓
Editable application → locked wheelhouse → qualification → distribution input
```

`distribution/` es una salida generada ignorada por Git; el código fuente canónico de generación reside en `tooling/distribution/web/`. El generador no sobrescribe un destino preexistente. `manifest.json` registra los archivos y hashes SHA256.

## CURRENT implementado

- Base: `src/application`, `pages/home.py`, módulo externo de ejemplo con `module.py`, `callbacks.py`, `pages/overview.py` y CSS registrado por `AssetLayer`.
- Overlay ADA: `create_composition(binding)` delega en `create_local_operational_composition()` y agrega el módulo externo; arranca mediante `run_operational_application(composition_factory=...)`. No duplica bootstrap ni Collector.
- Python operativo: **3.14.2** para los dos Starters; migración a 3.14.7 **PLANNED / DEFERRED** por decisión expresa.
- `build_wheelhouse.py`: exporta locks, construye wheels internos, descarga/valida wheels externos compatibles para runtime y build, registra versiones/hashes en manifest. Los wheels de terceros son archivos separados: no están incrustados en nuestros wheels.
- `qualify_starter.py` y `probe_starter.py`: SOURCE_SMOKE y PORTABLE offline, con checks de health, Home, ejemplo, layout, registro de página, callback HTTP y CSS HTTP.
- Dockerfile existente: multistage y offline, Python 3.14.2, usuario no privilegiado, healthcheck, puerto interno **8050**, `python -m application` y rechazo explícito de producción. El cambio a Gunicorn/8000 NO está implementado.
- ADA Generic resuelve publicaciones en un directorio externo mediante `APPLICATION_PUBLICATIONS_ROOT`.

## Evidencia y límites

**VERIFIED / CLOSED por terminal del usuario:** SOURCE_SMOKE PASS de Generic y ADA; 36 wheels Generic y 108 ADA; PORTABLE PASS de los dos en Python 3.14.2 mediante instalación offline y nueve checks. Las ejecuciones específicas del tooling informaron `11`, `8`, `16` y `18` tests aprobados durante sus incrementos.

**VERIFIED MANUAL / CONTAINER PARTIAL:** 5 tests del template Docker, 2 de publications root ADA; imágenes Generic y ADA construidas; ambas respondieron a `/health/live`; Generic entregó HTML `/example` con CSS referenciado.

**FINDING / OPEN:** Docker ADA respondió con HTML **Acceso denegado** a `/example`. El probe sintético no simulaba suficientemente solicitudes `Accept: text/html` propias de un navegador. Una prueba de página registrada no prueba autorización real de Navigation.

**OPEN — Manager visible y estilo real:** durante la qualification y en el contenedor ADA se configuró `ADA_MANAGER_PERSISTENCE_PROVIDER=disabled`. No se verificó Manager Home/header/sidebar, ni publicación/proyección/consumo de Navigation desde el Starter, ni recorrido visual completo de la identidad Atlanticus. El usuario identificó esta brecha. Ello no demuestra que Manager se haya eliminado del core: existe una composición opcional distinta.

## Fronteras posteriores separadas

- **PROPOSED / PLANNED:** un único Dockerfile local/productivo con Gunicorn y puerto interno `8000`; requiere un punto WSGI que conserve bootstrap, Collector e identidad/autorización correctos. Docker 8050 actual sigue CURRENT.
- **PLANNED:** plantillas inactivas y sin secretos `secrets.json`, `dev.mapping-env.csv`, `uat.mapping-env.csv`, `prd.mapping-env.csv`; el consumidor elige archivos activos y cualquier mapeo de variables debe quedar explícito.
- **PLANNED / UNVERIFIED:** Compose local Cosmos/Azurite, provisionamiento y recovery durable; host Entra productivo, CI y Azure deployment.

## Contratos congelados

No framework adicional al existente, Generic independiente de ADA, Manager/Navigation con sus ownership, permisos sin bypass, no fallback Source al leer Tool Projection runtime, Collector único, `distribution/` como output, Python 3.14.2 CURRENT. Atlanticus produce artifact/distribution input; DevOps externo posee el pipeline.

**Próximo foco propuesto, no implementado en este cierre:** recorrido integrado Manager–Navigation–visual qualification en el Starter apropiado, preservando shells separados.
