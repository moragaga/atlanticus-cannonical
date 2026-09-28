# ADA Generic — Current Composition

Estado: **CURRENT / CORE STAGE 1 CLOSED / ADA WEB COMPOSE DURABLE VALIDATED LOCALLY / NEW ADMIN WORK PLANNED**

Corte inspeccionado: `moragaga/atlanticus@ebc7a8bf8d49e931fd4e2487dac5ee036011a0a5` (2026-09-27). Este HEAD contiene un incremento posterior de Alarm, separado de este cierre. Las pruebas reportadas abajo corresponden a sus respectivos checkpoints, **no** constituyen ejecución global del HEAD nuevo.

## Composition CURRENT

ADA Generic compone branding, shell operacional, Navigation, alarm surfaces, content state, OperationalRenderBinding estructural, runtime experience, global indicators, time status y consumos configurados; cada capability conserva ownership independiente. Atlanticus genérico no depende de ADA.

```text
AdaGenericSettings
→ ToolPersistenceComposition (source local/blob; projection local/cosmos)
→ resolve_operational_tool_projection()
→ READY | UNCONFIGURED | UNAVAILABLE | INVALID
→ ADA Generic Web
```

Runtime consume la Projection durable y no requiere Tool Source en su lectura ordinaria. Con Tool READY y KPI Delivery Cosmos configurado puede adjuntar Collector. Collector ejecuta polling lazy/worker-local y navegador consume caché, no Cosmos inline. `OperationalRenderBinding` comunica estructura y no KPI state ni render universal. `1 ToolComponent → 1 logical/browser store`; los subcomponentes no crean stores. No reabrir Stage 1 sin finding.

## Manager + identidad CURRENT

Manager se integra al disponer de stores/dependencies. `ADA_MANAGER_PERSISTENCE_PROVIDER`: `auto|local|durable|disabled`, con `auto+local → local`, `auto+production → disabled`. Productivo durable exige un IdentityProvider real provisto por el host; el Starter no implementa Entra automáticamente.

Navigation puede consumir la proyección compartida desde la composición con Manager. Home/sidebar/header administrativos pertenecen al Manager y no se fusionan con el shell operacional. `administrative_override` tiene precondiciones propias y no es autorización Manager genérica; no se concede a Guest por falta de configuración.

Se conservan los cierres visuales locales previos: header ADA + Atlanticus con `Usuarios`, sin nombre de usuario visible; navegación local reconocida de Jane (rosa `#C85D91`) y John (azul `#3778C2`) en avatar e insignia Local. La selección local automática se resuelve una vez por arranque si no se fija subject. No extrapolar estas propiedades a Entra productiva.

## Web Starter distribuido CURRENT — evidencia más reciente

Código inspeccionado en `tooling/distribution/web/starter/ada/`:

```text
application/runtime.py           startup por worker + recursos con ExitStack
application/local_resources.py   preparación de recursos locales
 deployment/compose/{infra,web,full}.yaml
 tooling/project.py             compose {build,up,down,logs,ps,prepare}
```

`full` configura Manager `durable`, Source Blob vía Azurite y Projection Cosmos vía emulador, recursos/volúmenes externos persistentes y web vía Gunicorn. `infra`, `web` y `full` son perfiles reales CURRENT del Starter; no atribuirlos al generador separado `deployment/local/generate_compose.py` de procesos backend.

**VERIFIED AUTOMATED (usuario):** en el entorno con Docker, pruebas específicas de Compose `33 passed in 0.70s` tras los tres patches Compose. No representa tests completos monorepo.

**VERIFIED MANUAL (usuario + consola aportada):** Starter ADA generado; artifact construido y `qualify_distribution.py` informó `PRECHECK_PASS`; Compose `full` construyó la imagen y levantó Cosmos emulador y Azurite; `resources` creó/preparó seis contenedores Cosmos, Gunicorn arrancó tres workers. El usuario confirmó que el entorno funcionó. El PRECHECK reportó explícitamente `image_build=UNVERIFIED` y `runtime=UNVERIFIED`: las observaciones manuales posteriores avalan el arranque, pero no convierten esas salidas PRECHECK en un gate runtime automatizado.

**UNVERIFIED específicamente:** una secuencia reproducible publicada que compruebe que los mismos Sources y Projections sobreviven a `down/up` y que Navigation/Tools vuelven a leerse sin reproyección; Azure productivo, Entra, toda la matriz `infra/web`, todas las rutas HTML/permisos y tests globales. No atribuir esas verificaciones por deducción.

## Etiquetas de proveedores — último correctivo

La UI genérica utiliza nombres visuales inyectados; el patch ADA `Atlanticus_ADA_Manager_Provider_Labels_01` conectó nombres a la selección explícita del Manager: `local -> Local Source / Local Projection` y `durable -> Blob Storage / Cosmos DB`, sin modificar contratos de Source/Projection. Aplica a módulos integrados por la composición (Navigation, Tools, Profiles, Access y KPI).

**VERIFIED (usuario):** `git apply --check`, `git apply`, `git diff --check` sin errores mostrados; primera invocación pytest reportó once puntos exitosos; la segunda mostró `6 passed`. El HEAD posterior del usuario fue `de3ae01147dbec6d734253def5586ac92524fec0`. **UNVERIFIED:** evidencia visual desde imagen regenerada o requalification global de ese HEAD.

## Fronteras nuevas NO implementadas

- `USERS-PROJECTION-RECOVERY-001`: verificar Source aprobado de Users contra Cosmos y reconstruir la proyección de forma explícita, especialmente entre ambientes o tras manipulación externa.
- Página **aislada fuera de Manager** para proyectar las configuraciones ya disponibles en Storage, protegida por credenciales de proyecto y sin permitir administración convencional de módulos.
- Página ADA de Identificación operacional (cargo de catálogo manual, área Mina/Planta, grupo 1-4, los tres opcionales). No modificar el UserRecord genérico ni mezclar atributos operacionales con Access.
- Eventual `extra` sólo en Cosmos y posible evolución de Access son ideas **PLANNED / NO CONTRACT**, no parte de estas tres capacidades.

## Conflictos/pendientes conocidos

La baseline Project pretende Python `3.14.7` / `python:3.14.7-slim-trixie`, mientras metadata y plantilla Docker observadas del Starter siguen en `3.14.2` / `python:3.14.2-slim-bookworm`. No alterar fuera de un incremento dedicado. La canonical histórica aún describía Cosmos/Azurite/Compose integrados como PLANNED: queda SUPERSEDED para arranque local confirmado y tests Compose reportados, no para producción ni recuperación E2E tras reinicio.

## Estado

```text
ADA GENERIC CORE + COLLECTOR STAGE 1             CLOSED / CURRENT
LOCAL MANAGER UI/NAVIGATION PREVIOS              CLOSED / VERIFIED MANUAL
ADA STARTER COMPOSE FULL / RUNTIME STARTUP      CLOSED / VERIFIED MANUAL + 33 TESTS
PROVIDER LABELS PATCH                         CURRENT / TESTS VERIFIED / VISUAL UNVERIFIED
END-TO-END DURABLE RECOVERY AFTER RESTART       UNVERIFIED / SEPARATE
PRODUCTION ENTRA / FULL PIPELINE / PYTHON ALIGNMENT UNVERIFIED / SEPARATE
USERS-PROJECTION-RECOVERY-001                   PLANNED / NEXT
```
