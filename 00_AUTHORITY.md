# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Checkpoint CURRENT verificado para este cierre:
  `fbef06a8a0a587571527d9ecf131c73c5fc5f01a`
- Parent inmediato:
  `9f12c41a23d69784c7c5b775a4093a94ac654d55`
- Tree:
  `fc8c293f617aca4a53d89f687a22728e9d0fdcca`

El checkpoint CURRENT contiene, entre otros hitos ya cerrados:

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT

TOOLS-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-BOUNDARY-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CAPABILITY-EXTRACTION
CLOSED / VERIFIED / CURRENT

PROFILES-INDEPENDENT-SOURCE-LIFECYCLE
CLOSED / VERIFIED / CURRENT

USERS-PERSISTED-DATA-CUTOVER
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROFILES-CONFIGURATION
CLOSED / VERIFIED / CURRENT

NONPROMOTED-ACCESS-SEMANTICS-CORRECTION
CLOSED / VERIFIED / CURRENT

NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
CLOSED / VERIFIED / CURRENT

MANAGER-AUTHORIZATION-SEMANTICS-ALIGNMENT
CLOSED / VERIFIED / CURRENT

MANAGER-ACTIVE-WORKFLOW-CALLBACK-CARDINALITY
CLOSED / VERIFIED / CURRENT

CONFIGURATION-UI-COMPOSITION-RECOVERY
CLOSED / VERIFIED / CURRENT

GENERIC-WEB-PAGINATION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Existe un conflicto implementado todavía abierto en un consumer standalone:

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

`web/compositions/navigation-manager` llama `ManagerAuthorizationPolicy.can_access(...)`,
pero el contrato CURRENT de `ManagerAuthorizationPolicy` expone `can_view(...)`.
No tratar ese consumer como CLOSED hasta corregirlo y verificarlo.

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Checkpoint inspeccionado antes de este reemplazo:
  `4e59aa1e5160ff827ca6767fe178d3b45f4bc30d`

`atlanticus-cannonical:main` es autoridad documental vigente, subordinada a
`atlanticus:main` cuando la implementación publicada demuestra un estado posterior.

## Referencias históricas

`moragaga/atlanticus-decisions` es **HISTORICAL**.

Checkpoint inspeccionado:

```text
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

Puede aportar rationale, UX previamente aprobada y evidencia histórica. No puede reemplazar
`atlanticus:main` ni `atlanticus-cannonical:main`.

Durante este cierre no se encontró un registro histórico relevante que contradiga el
cutover de paginación genérica. Eso no convierte `atlanticus-decisions` en autoridad.

## Jerarquía

1. `atlanticus:main`: realidad implementada.
2. `atlanticus-cannonical:main`: contratos, fronteras, roadmap y estado vigente.
3. Qualification y tests vigentes: evidencia de propiedades demostradas.
4. Decisiones explícitas del Project todavía no formalizadas en canonical: delta temporal.
5. `atlanticus-decisions`: referencia histórica cuando sea útil.
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

Global Users
standalone / generic

Users Configuration Source
REMOVED

Users generic Projection
REMOVED

Users Manager Source/Projection module
REMOVED

Managed global authority
basic | root

local
runtime-only authority

Profiles
Atlanticus generic first-class capability

ADA Access
ADA-specific capability

Navigation
Atlanticus generic capability

Navigation durable authorization
allowed_profiles = profile keys

Navigation Configuration -> Profiles core
ALLOWED / CURRENT

Navigation -> Users
FORBIDDEN

Navigation -> ADA Access
FORBIDDEN

Navigation -> Profiles Configuration
FORBIDDEN
```

## Manager authorization CURRENT

Contrato CURRENT:

```text
ManagerModule.access_key: str | None
ManagerAuthorizationPolicy.can_view(principal, module)
```

`DefaultManagerAuthorizationPolicy` concede acceso únicamente cuando:

```text
module.access_key is not None
AND
module.access_key in principal.access_keys
```

No conceden autoridad implícita:

```text
principal.is_local
'administrator' in principal.profile_keys
```

`is_local` permanece metadata/contexto de runtime, no permiso.

El mismo permiso funcional del módulo protege acceso al módulo y las operaciones internas
del workflow Manager. No existen permisos separados Manager para `validate`, `publish` o
`project`.

ADA Configuration Manager CURRENT declara:

```text
navigation.manage
tools.manage
kpis.manage
```

El runtime local recibe esos access keys explícitamente.

## Users / Profiles / Access / Navigation CURRENT

Promotion de Users no habilita el ingreso a la aplicación.

```text
authenticated identity + no promoted UserRecord
→ READY
→ deterministic user_id
→ no UsersRuntime EffectiveUser

authenticated identity + promoted enabled UserRecord
→ READY
→ EffectiveUser available

authenticated identity + promoted disabled UserRecord
→ USER_DISABLED
→ 403
```

`USER_NOT_PROMOTED` no existe en `AccessStatus` CURRENT.

Navigation Configuration consume directamente Profiles core:

```text
ProfileCatalog
ProfileDefinition
NavigationProfileCatalogProvider = Callable[[], ProfileCatalog]
```

Sin provider, Navigation no inventa perfiles base. Fallos del provider no se silencian.

## Paginación Web CURRENT

El comportamiento transversal de paginación pertenece a Atlanticus:

```text
atlanticus.web.pagination
├── DEFAULT_PAGE_SIZE = 10
├── ALLOWED_PAGE_SIZES = (10, 20)
├── PageRequest
├── Page
└── paginate_items(...)
```

`Page` contiene únicamente registros reales.

No pertenecen al contrato genérico:

```text
markup Dash del paginador
CSS del paginador
placeholders visuales
sort/filter/search
SortDirection
```

Cada presentación conserva ownership de su UI. La presentación de paginación que hoy usa
ADA permanece en `ada.web.configuration.presentation` y consume el contrato genérico.

El contrato legacy:

```text
ada.web.configuration.pagination
```

fue removido sin aliases, shims ni reexports de compatibilidad.

## Superficies administrativas CURRENT

ADA Configuration Manager CURRENT compone:

```text
navigation
tools
kpis
kpi-definitions
```

Ausencia CURRENT verificada:

```text
Profiles Configuration UI
Users Administration UI
ADA Access Configuration UI
```

Esto no autoriza inventar UI ni contratos. Los dominios/backend existentes deben ser la
fuente de la futura superficie.

## Siguiente foco único recomendado

```text
PROFILES-CONFIGURATION-EDITOR-CONTRACT
PLANNED / NEXT
```

Debe consumir `ProfilesConfiguration`, Profiles core/Source lifecycle y
`atlanticus.web.pagination` sin crear UI transversal nueva.

La Web surface de Profiles queda separada como incremento posterior.
