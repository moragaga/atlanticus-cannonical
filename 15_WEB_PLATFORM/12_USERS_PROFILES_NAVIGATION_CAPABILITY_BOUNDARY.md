# Web Platform — Users / Profiles / Access / Navigation Capability Boundary

Estado: **CURRENT / NAVIGATION OPERATIONAL CONSUMER REFINED 2026-09-25**

Autoridad implementation inspeccionada para este delta:
`moragaga/atlanticus@a6061ffed59c8b04e64b0a7fdc17050ef463c850`.
El resto de los contratos Users/Profiles/Access del checkpoint canónico previo
permanece congelado: no se reabre desde este incremento.

## Propósito y propiedad

```text
Atlanticus Global Users           identity + lifecycle + user → profile_key
Atlanticus Generic Profiles      definition + catalog + Source + Projection
ADA Access                        declared access keys + profile_key → access_keys
Atlanticus Generic Navigation    route structure and profile visibility
Atlanticus Manager               own administrative shell and authorization
ADA Generic                       explicit composition/consumer of the above
```

Atlanticus Core no depende de ADA. Navigation Core/Configuration no depende de Users ni
ADA Access; Navigation Configuration no depende físicamente de Profiles. ADA composition
puede conectar capacidades mediante contratos públicos sin traspasar su ownership.

## Users CURRENT — contrato conservado

Persistente/efectivo:

```text
UserRecord.profile_key
EffectiveUser.profile_key
```

Profiles determina el catálogo; Users controla qué perfiles pueden asignarse a usuarios
administrados. System profiles: `basic`, `root`, `guest`, `local`.

```text
basic                      assignable managed profile
root                       assignable managed profile
configured custom profile  assignable managed profile
guest                      transient/pending representation; NOT managed assignment
local                      local runtime only; NOT managed assignment
```

`normalize_managed_profile_key('guest')` y la representación
`UserRecord(profile_key='guest')` son válidos en estados transitorios. La operación
`require_managed_profile('guest', ...)` lo rechaza y
`available_managed_profiles` excluye `guest` y `local`.
No trasladar esa prohibición al constructor de `UserRecord`.

### Users Administration

```text
UsersAdministrationService
├── discover
├── promote
└── update
```

Promoción V1: un usuario por vez, acción explícita `Promover`, sin batch.
Update V1: identidad informativa/no editable; perfil y enabled editables;
`Guardar` persiste inmediatamente. No existe borrador global Users.

Persistencia lógica implementada:

```text
promote → UsersRegistryStore.replace → UsersAdministrationStore.create
update  → UsersRegistryStore.replace → UsersAdministrationStore.replace
```

La qualification real del wiring Blob/Cosmos de Users no se deduce de las pruebas
in-process. Users integra Manager como `ManagerEntry`, sin Source/Projection sintéticos.

### Users UI

```text
Control de usuarios
├── Usuarios          (default)
└── Por promover
```

Una sola vista activa. Paginación `atlanticus.web.pagination` con `10/20`, navegación
numerada y viewport estable. Tabs según convención visual Atlanticus.
Editor propio de capability con backdrop y acciones close/cancel/save; no trasladar
ownership a `dbc.Modal` si abandona la frontera de la capability.
Pulido visual residual: diferido/no bloqueante.

## Profiles CURRENT — contrato conservado

Profiles es capability genérica y posee su catálogo/UI.

- System profiles: `basic`, `root`, `guest`, `local`.
- Apariencia normal: inicial mayúscula; `local`: iniciales nombre/apellido según identidad.
- Source/Projection labels inyectados por composition.
- Editor capability-local; paginación `10/20` con `atlanticus.web.pagination`.

El catálogo no obliga a todos los consumidores a ofrecer todos los perfiles como opciones.

## ADA Access CURRENT — contrato conservado

```text
profile_key → access_keys
```

No crear persistencia global `user → access_keys` en Users.
La autorización Manager sobre un módulo no se obtiene por tener acceso a una ruta
operacional de Navigation.

## Navigation Configuration CURRENT

Contratos públicos independientes:

```text
NavigationConfigurationCatalog
NavigationProfileOption(key, label)
NavigationProfileOptionsProvider (optional)
```

Una aplicación que conoce Profiles puede adaptar
`ProfileCatalog → tuple[NavigationProfileOption, ...]` sin acoplar Navigation a Profiles.

ADA Navigation Configuration ofrece `basic`, `guest` y perfiles configurados;
no ofrece `root` ni `local` en la selección de perfiles de rutas. `guest` es
válido como criterio de visibilidad Navigation pero no como asignación final Users.

## Navigation access semantics — CURRENT / REFINED

Para principal ordinario:

```text
enabled = False                      DENY
enabled = True, allowed_profiles=()  PUBLIC WITHIN NAVIGATION AUTHORIZATION
enabled = True, non-empty profiles  require principal.access_key membership
```

`principal.unrestricted` elude la restricción de perfil de rutas habilitadas,
**no** habilita rutas deshabilitadas ni concede acceso a rutas no registradas.

Excepción contractual implementada en Navigation Core:

```text
principal.administrative_override == True
→ Navigation page-document authorization permits disabled and unregistered paths
```

Es una excepción de recuperación, no un modo normal. ADA Generic la obtiene sólo
del contexto de principal administrativo verificado:

```text
managed root + not local                    → override
trusted local principal + local environment → override
ordinary / unknown principal               → no override
```

La excepción de Navigation no concede `navigation.manage`, otros permisos Manager ni
acceso general a API/servicios. Menu sigue excluyendo rutas deshabilitadas. Las rutas
externas no se convierten en documentos internos protegidos por el matcher.
La autorización de documentos HTML excluye según el código `_dash`, assets, health,
API y `.auth` del middleware específico Navigation; otros controles conservan ownership.

## ADA Generic integration CURRENT

- Navigation base se monta sin Identity obligatoria.
- Sin proyección, el menú es vacío; Home conserva acceso.
- Al integrar Manager, Navigation consume `navigation_projection_store` compartida,
  usando `NAVIGATION_SOURCE_KEY`, no un menú fijo duplicado.
- Si Manager no puede resolver principal por dependencias realmente no disponibles,
  el código usa principal público en el caso de errores clasificados. No ampliar
  silenciosamente esa captura a errores lógicos o autorizaciones denegadas.
- Local Identity sólo se instala cuando la composition lo necesita según sus reglas.

La UI ADA Navigation usa controller `dcc.Location` + último pathname fuera del
Offcanvas. Los triggers no tienen `title` HTML nativo y conservan un texto oculto
accesible. El callback detecta pulsación y cambios efectivos de ruta, sin tratar
hidratación como navegación real.

## Manager vs Navigation

`/manager` es Home real, gobernada por `ManagerModuleRegistry`.
Manager conserva Home, sidebar, grupos, items, `access_keys` y workflows.
Navigation operacional no administra ese registry.

ADA Configuration Manager compone:

```text
Administración:
- Users

Configuraciones:
- Profiles
- Accesos
- Navigation
- Tools
- KPI
- KPI Definition
```

La UI específica permanece en cada capability; la metadata visible puede
inyectarse por composition sin transferir ownership.

## Qualification de este delta

**VERIFIED STATIC:** commit `a6061ffe` contiene Navigation Core authorization,
consumidor ADA Generic, callback/controller corregido y tests correspondientes.

**VERIFIED AUTOMATED PREVIOUS:** antes del commit final, se reportaron ADA Generic
`169 passed` y, posteriormente, `172 passed` con Ruff verde. Shell informó
`8 passed, 1 skipped` antes de la última corrección cliente. No clasificar como
qualification completa automatizada de `a6061ffe`.

**VERIFIED MANUAL (usuario):** Navigation local guardada, publicada, proyectada y
visible desde Home; menú funcional tras corrección final.

**UNVERIFIED:** Blob/Cosmos real con reinicio, navegadores/responsive, Entra y
pruebas completas del commit final.

## Estado refinado

```text
NAVIGATION-STANDALONE-CONFIGURATION-CUTOVER           CLOSED / CURRENT
NAVIGATION-PUBLIC-ACCESS-CONTRACT                    CLOSED / CURRENT
NAVIGATION-PROFILE-OPTIONS-DECOUPLING                CLOSED / CURRENT
NAVIGATION-OPERATIONAL-AUTHORIZATION-INTEGRATION     CLOSED / CURRENT
NAVIGATION-MANAGER-CONSUMER-ALIGNMENT                CLOSED / CURRENT
NAVIGATION-LOCAL-PUBLISH-PROJECT-CONSUME             CLOSED / VERIFIED MANUAL
NAVIGATION CLIENT CODE CORRECTION                     CURRENT / USER-REPORTED FUNCTIONAL
MANAGER-REAL-PERSISTENCE-QUALIFICATION               PLANNED / UNVERIFIED
PRODUCTION-IDENTITY-PROVIDER / ENTRA                 PLANNED / UNVERIFIED
```

La etiqueta histórica `NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT BLOCKED`
queda **SUPERSEDED** para el código presente. La formulación universal
`enabled=False → deny` se restringe al principal ordinario: `administrative_override`
es una excepción formal de código. No inventar otras excepciones.

## Testing boundary

KEEP: comportamiento, invariantes de dominio, pruebas de imports/fronteras reales,
callbacks funcionales, persistencia, recovery y errores importantes.

DO NOT ADD: pruebas CSS visuales, snapshots de geometría/responsive, assertions
únicamente sobre estructura visual, existencia/ausencia interna de funciones
no contractuales o pruebas congelando implementación accidental.

## Reglas congeladas

```text
Atlanticus generic                                          REQUIRED
Users → profile_key                                         CURRENT
Users app-specific access                                   FORBIDDEN
Users Manager integration                                   ManagerEntry
Users synthetic Manager Source/Projection                   FORBIDDEN
Users global draft                                          FORBIDDEN
Users promotion V1                                          SINGLE USER
Users identity edit                                         FORBIDDEN
guest pending representation                                ALLOWED
guest managed assignment                                    FORBIDDEN
local managed assignment                                    FORBIDDEN
Profiles UI ownership                                      PROFILES CAPABILITY
ADA Access ownership                                       ADA-SPECIFIC
Navigation Configuration → Profiles core                    FORBIDDEN
Navigation optional profile options provider                CURRENT
Navigation → Users / ADA Access                             FORBIDDEN
empty allowed_profiles                                     PUBLIC WITHIN NAVIGATION
principal.unrestricted                                      ENABLED PROFILE BYPASS ONLY
principal.administrative_override                           AUTHORIZED RECOVERY EXCEPTION
Navigation Manager access_keys                              INDEPENDENT
LEGACY SHIMS / TEMPORARY ADAPTERS / DOUBLE CONTRACT          FORBIDDEN
LEGACY SCHEMA READERS                                       FORBIDDEN
```

## Abiertos explícitos y separados

- Users tests finales transversales y algunos detalles visuales: UNVERIFIED/DEFERRED.
- Manager responsive audit y Web test cleanup: DEFERRED.
- Manager real durable persistence: PLANNED/UNVERIFIED.
- Producción Entra/Graph concreta: UNVERIFIED.
- Metadata Python `3.14.7` vs implementaciones `3.14.2`: CONFLICT.
- CI remoto, Ruff workspace y monorepo qualification: UNVERIFIED.
