# ADA Generic — Current Composition

Estado: **CURRENT / CORE STAGE 1 CLOSED / LOCAL MANAGER VISUAL CLOSED / GENERATED STARTER INTEGRATION OPEN**

Última inspección estática para el cierre local: `moragaga/atlanticus@392ee281a32396516fb08c23c63514d8cbdb3489` (2026-09-27). Las pruebas históricas de Starter y Docker se realizaron sobre checkpoints anteriores y no se atribuyen al HEAD de este cierre.

## Composición y bootstrap CURRENT

ADA Generic compone branding y shell operacional, Navigation, alarm surfaces, content state, render binding, operational state, runtime experience, global indicators, time status y los consumos configurados. Cada capability conserva su ownership independiente; Atlanticus core no depende de ADA.

```text
AdaGenericSettings
→ Tool persistence settings
→ optional Storage / Tool Projection Cosmos clients
→ ToolPersistenceComposition
→ resolve_operational_tool_projection()
→ READY | UNCONFIGURED | UNAVAILABLE | INVALID
```

Un estado distinto de READY conserva Web base y diagnósticos. Runtime consume Tool Projection durable, no Source ni fallback implícito. Si Tool está READY y KPI Delivery Cosmos está configurado, se adjunta el Collector existente. Polling lazy/worker-local; navegador consume caché, no Cosmos inline. `OperationalRenderBinding` sólo comunica estructura y no posee estado ni render universal. `1 ToolComponent → 1 dcc.Store`; los subcomponents no generan stores adicionales.

## Manager + Navigation CURRENT en core

- Home pública no obliga a tener identidad; Navigation y su middleware siguen sus contratos de autorización.
- Manager se integra sólo al disponer de stores/dependencies. `ADA_MANAGER_PERSISTENCE_PROVIDER` admite `auto|local|durable|disabled`; `auto + local → local`, `auto + production → disabled`; `durable` productivo requiere host con IdentityProvider real y no se simula mediante identidad local.
- Con Manager activo, Navigation puede consumir la proyección compartida; Manager Home/sidebar/header son propios y no se fusionan con el shell operacional ADA.
- Root autorizado/local confiable pueden recibir `administrative_override` sólo conforme al contrato. No conceder override a invitados por ausencia de datos.

## Local Identity + representación — cierre nuevo

Sin `ATLANTICUS_LOCAL_IDENTITY_SUBJECT_ID`, el CLI usa `select_local_user()` (Jane o John, selección aleatoria por arranque). Establecerlo explícitamente sólo sirve para arranques deterministas. `manager_navigation_principal` obtiene, por subject local verificado y perfil local, **avatar e insignia Local** desde `atlanticus.web.users.local`: Jane rosa `#C85D91`, John azul `#3778C2`. El fallback de desconocidos y públicos y las reglas para usuarios administrados permanecen. Esto fue **VERIFIED STATIC** en `main` y **VERIFIED MANUAL** visualmente por el usuario. La salida de la suite corregida no se aportó en este chat.

En el Manager ADA local se aceptó visualmente el header con marcas ADA y Atlanticus, enlaces `Manager Home` y retorno cuando aplica, etiqueta `Usuarios` y **sin nombre de usuario en el header**. La supresión es exclusivamente visual.

## Composition externa del Starter — brecha aún OPEN

`tooling/distribution/web/starter/ada/src/application/composition.py` añade el módulo externo a la composición operacional existente; `application/runtime.py` usa `run_operational_application(composition_factory=...)`, sin segundo bootstrap, Manager ni Collector. La qualification histórica de `qualify_starter.py` y Docker ADA utilizó `ADA_MANAGER_PERSISTENCE_PROVIDER=disabled`. Por tanto, **no demuestra** Manager/header/sidebar en ese Starter ni publicación/proyección de Navigation desde él.

En aquella prueba Docker, la solicitud HTML a `/example` mostró `Acceso denegado`, aunque la probe sintética registró PASS. El middleware de Navigation autoriza rutas según definición proyectada; registrar una página Dash no otorga acceso. Resolverlo en qualification distribuida mediante solicitudes HTML reales, sin menú hardcodeado ni bypass de identidad.

## Estado de calificación

```text
ADA GENERIC CORE / COLLECTOR STAGE 1     CLOSED / CURRENT (hitos previos)
LOCAL MANAGER HEADER + USERS LABEL        CLOSED / VERIFIED MANUAL
LOCAL JANE/JOHN NAVIGATION COLORS         CLOSED / VERIFIED MANUAL
STARTER ADA MANAGER + NAVIGATION HTML     OPEN / UNVERIFIED
GENERIC+ADA STARTER SOURCE/PORTABLE       CLOSED / VERIFIED MANUAL HISTÓRICO
DOCKER BUILD + LIVENESS ANTIGUO          VERIFIED MANUAL / PARTIAL
COSMOS/AZURITE + RESTART                 PLANNED / UNVERIFIED
ENTRA PRODUCTIVA + PIPELINE              PLANNED / UNVERIFIED
```

No reabrir Collector ni convertir este cierre local en una qualification de distribución.
