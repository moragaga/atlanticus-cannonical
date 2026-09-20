# Web Platform — Source Ledger

Estado: **AUDIT LEDGER**

## Corte CURRENT

```text
moragaga/atlanticus@783d3578da52aeb5cf831999a7717dc8b79f2fb0
```

Parent:

```text
e0dca2d9f9e8db9551b8cee45a37cd1ce3dd4bd5
```

Tree:

```text
5ed091d5477b8ca041ddd669de8217028ec72f35
```

## Users

CURRENT packages relevantes:

```text
web/capabilities/users/core
web/capabilities/users/activity
web/capabilities/users/blob
web/capabilities/users/cosmos
web/compositions/users-manager
```

CURRENT ownership:

```text
identity
lifecycle
user -> profile_key
```

No existe CURRENT:

```text
authority_key
users/configuration
Users generic Projection
Users Manager Source/Projection module
```

`UsersAdministrationService` es el lifecycle administrativo.

Users Manager integra ese service mediante `ManagerEntry`.

Capability:

```text
users.manage
```

Web surface CURRENT administra únicamente:

```text
profile_key
enabled
```

Identity/directory facts son read-only.

## Profiles

CURRENT:

```text
profiles/core
profiles/configuration
profiles/projection-local
profiles/projection-cosmos
web/compositions/profiles-manager
```

Profiles es generic Atlanticus y posee `ProfileCatalog`.

Users consume Profiles core para validar/seleccionar profiles.

## ADA Access

CURRENT packages:

```text
scopes/ada/web/access/core
scopes/ada/web/access/configuration
scopes/ada/web/access/projection-local
scopes/ada/web/access/projection-cosmos
```

Models verificados:

```text
ProfileAccessGrant(profile_key, access_keys)
EffectiveAdaAccess(profile_key, access_keys)
AdaAccessConfiguration(profile_access)
```

Ownership:

```text
profile_key -> access_keys
```

`access_key` se normaliza como string con formato:

```text
[a-z0-9][a-z0-9._-]*
```

No existe catálogo de definiciones de acceso CURRENT.

Source:

```text
AdaAccessSourceService
schema_version = 2
resource = access/configuration.json.gz
```

Projection:

```text
AdaAccessProjectionBuilder
exactly one Profiles dependency
dependency must match active exact Profiles ProjectionTarget
payload = AdaAccessConfiguration
```

Persistencia:

```text
ProjectionRecord[AdaAccessConfiguration]
recursive exact dependencies
projection-local
projection-cosmos
```

No reconstruir provenance desde Profiles CURRENT después de restart.

## ADA Access Web evidence

CURRENT no contiene package/directorio de Web UI Access.

Los `pyproject.toml` de Access core/configuration/projection-local/projection-cosmos no
declaran Dash ni una extra Web.

La historia inspeccionada de commits que tocaron `scopes/ada/web/access` desde su creación
muestra únicamente domain/configuration/source/projection/persistence; no muestra una Web
surface Access.

Por tanto:

```text
ADA Access Web surface
NOT IMPLEMENTED / PLANNED
```

No asumir callbacks, layout, route, CSS, IDs ni composition existentes.

## Navigation

CURRENT:

```text
Navigation Configuration -> Profiles core
Navigation -> Users FORBIDDEN
Navigation -> ADA Access FORBIDDEN
```

`allowed_profiles` contiene profile keys.

## Manager

CURRENT core distingue:

```text
ManagerModule
ManagerEntry
```

Registry:

```text
modules
entries
items
```

Authorization:

```text
can_view(principal, ManagerModule | ManagerEntry)
```

ADA Configuration Manager CURRENT compone:

```text
Administration:
- Users

Configuration:
- Profiles
- Navigation
- Tools
- KPI
- KPI Definition
```

## Qualification Users Manager observada

Antes del publish funcional:

```text
23 targeted passed
26 ADA Configuration Manager passed
46 Users core passed
62 Manager passed
1 users-manager passed
109 Web scoped combined passed
git diff --check PASS
uv lock / uv sync PASS
```

El commit `e0dca2d9...` publicó la implementación.

El commit CURRENT `783d3578...` elimina únicamente
`web/compositions/users-manager/uv.lock`.

No se usa como evidencia el intento de ejecutar pytest desde la raíz del monorepo, porque
recolectó proyectos independientes con entornos/dependencias distintos y produjo colisiones
de módulos de test.

## Requirement próximo

Para ADA Access Manager, el producto requiere un flujo manual/controlado:

```text
1. definir/crear un acceso
2. disponer de un identificador estable
3. asignar acceso(s) a Profile(s)
4. resolver Profile -> access identifiers
5. el desarrollador referencia manualmente el identificador en la Web/funcionalidad
```

Los pasos 1 y 2 no tienen modelo CURRENT equivalente.

No inventar su shape antes del diseño.

## Finding pendiente

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

No añadir alias.
