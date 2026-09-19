# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global

| Decisión | Estado |
|---|---|
| Python 3.14.7 | DECIDED / LOCALLY USED / METADATA NOT YET GLOBALLY ALIGNED |
| `python:3.14.7-slim-trixie` | DECIDED / NOT YET QUALIFIED GLOBALLY |
| `uv`, no pip normal | CURRENT |
| Definir contratos antes que consumidores | CURRENT |
| Backend antes que frontend | CURRENT |
| Cutover raíz limpio | CURRENT |
| No crear shims/adapters/aliases temporales para legacy | FROZEN |
| No conservar doble contrato | FROZEN |
| Tests no son autoridad sobre contratos SUPERSEDED | FROZEN |
| Un consumer puede quedar temporalmente roto durante un root cutover | FROZEN |

## Regla universal de cutover

```text
LEGACY
REMOVE

ADAPTERS / SHIMS / ALIASES
FORBIDDEN

DOBLE CONTRATO
FORBIDDEN

OLD SCHEMA READERS IN CURRENT RUNTIME
FORBIDDEN

CONTRATO FINAL
Responsabilidad real del dominio; infraestructura genérica sólo donde aplique
```

No forzar Source/Projection/Manager sobre una entidad que no sea configuration lifecycle.

## Source / Projection

Permanecen CURRENT/FROZEN:

```text
Source generic -> web/capabilities/source
Projection exact-release -> web/capabilities/projection/core
release identity != content hash
ProjectionTarget = SourceKey + SourceReleaseRef + dependencies
project(target) no relee current
Manager no reconstruye ProjectionTarget desde revision
expected_source_revision REMOVED
```

## Manager generic contract

CURRENT:

```text
ManagerModule
├── source_key
├── source_service
├── source_reader_service
├── projection_service
├── draft_validation_service
├── source_history_service | None
└── access_key | None
```

SUPERSEDED / REMOVED:

```text
ManagerModuleAccess
workflow_service
exact_source_*
exact_projection_service
expected_source_revision
per-operation Manager access fields
```

### Manager authorization

Contrato CURRENT:

```text
ManagerAuthorizationPolicy.can_view(principal, module)
```

Política default:

```text
required = module.access_key
required is None -> deny
required in principal.access_keys -> allow
otherwise -> deny
```

No hay bypass de autorización por:

```text
is_local
administrator profile
```

`is_local` puede permanecer como contexto de runtime, pero no concede permisos.

Una capacidad funcional de módulo protege el workflow completo del módulo.

```text
navigation.manage
→ acceso funcional al módulo Navigation Manager

tools.manage
→ acceso funcional al módulo Tools Manager

kpis.manage
→ acceso funcional a KPI Configuration y KPI Definition en la composition ADA CURRENT
```

No reinterpretar estas keys como permisos separados de guardar/validar/publicar/proyectar.
Esas acciones son mecanismos internos del workflow del módulo.

### Manager callback transition

Cuando un callback pattern `ALL` queda temporalmente sin módulo resoluble durante una
transición de ruta, debe preservar estado mediante `PreventUpdate`, no fabricar listas de
cardinalidad cero que contradigan los outputs todavía montados.

## Global Users

Users es registry/lifecycle global y no Configuration Source.

```text
Users != Configuration Source
Users != Profile assignment
Users != Access configuration
Users != Navigation configuration
```

Managed authority:

```text
basic
root
```

Runtime local:

```text
local
```

No son Users authority CURRENT:

```text
guest
administrator
```

Users Administration UI debe consumir el lifecycle existente; no reintroducir Users
Source/Projection ni Users Manager configuration module.

## Profiles

Profiles es capability generic Atlanticus first-class.

```text
profiles/core
→ ProfileDefinition
→ ProfileCatalog

profiles/configuration
→ ProfilesConfiguration
→ Profiles Source lifecycle
```

UI pendiente no autoriza cambiar ese ownership.

## ADA Access

ADA Access es application-specific.

```text
user_id -> profile_keys
profile_key -> access_keys
```

No convertir ADA Access en dependency de Navigation.

UI pendiente debe permanecer ADA-owned.

## Navigation / Profiles

CURRENT:

```text
Navigation Configuration -> Profiles core
Navigation durable allowed_profiles = profile keys
Navigation -> Profiles Configuration FORBIDDEN
Navigation -> Users FORBIDDEN
Navigation -> ADA Access FORBIDDEN
```

No restaurar mini-modelo de perfiles dentro de Navigation.

## UI composition boundary

El Manager genérico posee shell/home/sidebar/workflow administrativo.

La configuración concreta de cada dominio permanece en su capability/domain.

La aplicación administrativa puede componer múltiples superficies, pero no debe fusionar
ownership de dominio para ganar simetría visual.

Estado CURRENT de superficies faltantes:

```text
Profiles Configuration UI
PLANNED

Users Administration UI
PLANNED

ADA Access Configuration UI
PLANNED
```

La creación/recuperación debe reutilizar lógica CURRENT y evidencia histórica verificable.
No inventar un framework UI nuevo antes de inspeccionar composiciones transversales ya
existentes o previamente implementadas.

## Testing

Automatizar comportamiento, contratos, invariantes, regresiones y flujos críticos.

No crear tests cuyo único objetivo sea fijar CSS visual, estructura interna o símbolos.
Visualizaciones perdidas se validan visualmente salvo que exista comportamiento funcional
automatizable.

## Conflict CURRENT conocido

`web/compositions/navigation-manager` usa `authorization.can_access(...)` aunque el
protocolo CURRENT sólo declara `can_view(...)`.

No crear alias `can_access` para conservar el consumer.

Debe alinearse directamente al contrato final cuando se trabaje esa composition.

## Siguiente foco

```text
CONFIGURATION-UI-COMPOSITION-RECOVERY
PLANNED / NEXT
```

Primero inventario y recuperación. Después, un solo incremento UI faltante por vez.
