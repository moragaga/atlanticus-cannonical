# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Checkpoint CURRENT verificado para este cierre:
  `a31fce11d26a7c0a554d82de1813a4311522919b`
- Parent inmediato:
  `90e89c376dfdfd182f0380b1d407127ecb7c9711`
- Tree:
  `737310de59774f3033607c1ef17c1c921efe1e09`

El checkpoint CURRENT conserva los hitos anteriormente cerrados de Manager generic,
Navigation, Tools, KPI Configuration, KPI Definition, Users registry/persistencia,
Profiles extraction/lifecycle, Manager authorization, UI composition recovery y generic
pagination.

Además contiene:

```text
PROFILES-CONFIGURATION-EDITOR-CONTRACT
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-WEB-SURFACE
CLOSED / VERIFIED / CURRENT

PROFILES-PROJECTION-CONTRACT
CLOSED / VERIFIED / CURRENT

PROFILES-MANAGER-COMPOSITION
CLOSED / VERIFIED / CURRENT

USERS-PROFILES-CONTRACT-REALIGNMENT
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROFILE-OWNERSHIP-REALIGNMENT
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROJECTION-CONTRACT
CLOSED / VERIFIED / CURRENT
```

Permanece un conflicto implementado previo:

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

`web/compositions/navigation-manager` no debe tratarse como alineado hasta que consuma
directamente el contrato CURRENT de autorización Manager. No crear alias/shim para
preservar el consumer.

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Checkpoint inspeccionado antes de este reemplazo:
  `a7adef2568d664ee31cb1b0eb1fe9f11ce2b9203`
- Tree inspeccionado:
  `9f8335f0de8c052e556c02be062757604c33bfcd`

`atlanticus-cannonical:main` es autoridad documental vigente, subordinada a
`atlanticus:main` cuando la implementación publicada demuestra un estado posterior.

## Referencias históricas

`moragaga/atlanticus-decisions` es **HISTORICAL**.

Puede aportar rationale y evidencia histórica. No puede reemplazar `atlanticus:main` ni
`atlanticus-cannonical:main`.

Durante este cierre no se verificó un decision record histórico que contradiga o reemplace
los contracts publicados en `atlanticus:main`.

## Jerarquía

1. `atlanticus:main`: realidad implementada.
2. `atlanticus-cannonical:main`: contracts, fronteras, roadmap y estado vigente.
3. Qualification y tests vigentes: evidencia de propiedades demostradas.
4. Decisiones explícitas del Project todavía no formalizadas en canonical: delta temporal.
5. `atlanticus-decisions`: referencia histórica.
6. Memoria/historial conversacional: pista, nunca autoridad suficiente.

## Clasificación obligatoria

```text
VERIFIED
INFERRED
ASSUMED
PROPOSED
UNVERIFIED
```

Estados:

```text
CURRENT
IN PROGRESS
PLANNED
SUPERSEDED
BLOCKED
CLOSED
```

Si implementación y canonical se contradicen, exponer el conflicto y actualizar canonical;
nunca retroceder implementación CURRENT para satisfacer documentación obsoleta.

## Git

Git es **READ ONLY** por defecto.

No crear commits, push, ramas, PR, issues ni mutaciones remotas sin autorización explícita.

## Continuidad congelada

No reabrir sin conflicto demostrado:

```text
LEGACY
REMOVE

ADAPTERS / SHIMS / ALIASES
FORBIDDEN

DOUBLE CONTRACT
FORBIDDEN

OLD SCHEMA RUNTIME READERS
FORBIDDEN

Manager exact/legacy dual contract
REMOVED

expected_source_revision
REMOVED

Users Configuration Source
REMOVED

Users generic Projection
REMOVED

Users Manager Source/Projection module
REMOVED
```

## Source / Projection CURRENT

```text
Source generic
web/capabilities/source

Projection exact-release
web/capabilities/projection/core

ProjectionTarget
SourceKey + SourceReleaseRef + dependencies
```

`project(target)` no reconstruye target desde una revision textual.

Dependencias exactas se modelan mediante `ProjectionTarget.dependencies` cuando existen
realmente.

## Manager authorization CURRENT

Contrato CURRENT:

```text
ManagerModule.access_key: str | None
ManagerAuthorizationPolicy.can_view(principal, module)
```

No conceden autoridad implícita:

```text
principal.is_local
administrator profile
```

El mismo permiso funcional del módulo protege el módulo y sus operaciones de workflow.

## Users / Profiles CURRENT

Profiles posee definición y catálogo de perfiles.

Users posee:

```text
user -> profile_key
```

Contrato CURRENT:

```text
UserRecord.profile_key
EffectiveUser.profile_key
```

SUPERSEDED / REMOVED:

```text
authority_key
authority.py
basic|root assignable-authority mini-contract
```

Managed users pueden referenciar perfiles existentes en `ProfileCatalog` salvo `local`.

`local` es runtime-only para identidades locales.

Persistencia CURRENT:

```text
Blob Users Registry schema 2
Cosmos Users schema 2
Users session snapshot v4
```

## Profiles CURRENT

```text
profiles/core
profiles/configuration
profiles/projection-local
profiles/projection-cosmos
web/compositions/profiles-manager
```

Profiles Projection materializa `ProfileCatalog`.

Los perfiles de sistema pertenecen al código de Profiles; configured profiles se conservan
en la persistencia de Projection.

La Web surface de Profiles y la composition Manager reusable están implementadas.

Esto no implica que una aplicación final administrativa ya componga todas las superficies
pendientes.

## ADA Access CURRENT

ADA Access es application-specific.

Ownership:

```text
profile_key -> access_keys
```

SUPERSEDED / REMOVED:

```text
user_id -> profile_keys
UserProfileAssignment
```

Contrato CURRENT:

```text
ProfileAccessGrant
EffectiveAdaAccess(profile_key, access_keys)
AdaAccessConfiguration(profile_access=...)
```

Source schema:

```text
ADA_ACCESS_SOURCE_SCHEMA_VERSION = 2
```

Projection CURRENT:

```text
AdaAccessProjectionBuilder
create_ada_access_projection_service
ProjectionRecord[AdaAccessConfiguration]
```

Dependencia exacta:

```text
Profiles ProjectionTarget
        ↓
ADA Access ProjectionTarget
```

ADA Access valida profile keys contra el `ProfileCatalog` de esa dependencia.

## Navigation CURRENT

Navigation Configuration consume Profiles core:

```text
ProfileCatalog
ProfileDefinition
NavigationProfileCatalogProvider
```

Durable authorization:

```text
allowed_profiles = profile keys
```

No depende de:

```text
Users
ADA Access
Profiles Configuration
```

## Paginación Web CURRENT

```text
atlanticus.web.pagination
DEFAULT_PAGE_SIZE = 10
ALLOWED_PAGE_SIZES = (10, 20)
PageRequest
Page
paginate_items(...)
```

No pertenecen al contract generic:

```text
markup Dash
CSS
placeholders visuales
sort/filter/search
```

## Modelo operacional

No asumir automatización total.

Procesos automáticos, semi-automatizados y manuales controlados pueden formar parte del
contract cuando se definan explícitamente.

## Siguiente foco único recomendado

```text
ADA-ACCESS-PROJECTION-PERSISTENCE
PLANNED / NEXT / DESIGN FIRST
```

Primero verificar contracts y providers existentes. No inventar package layout, serializer,
schema durable o Cosmos topology antes de cerrar diseño.
