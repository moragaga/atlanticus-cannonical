# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Checkpoint CURRENT verificado para este cierre:
  `3eb46dac80f23d438774e3afa39999dc96f592d7`
- Parent inmediato:
  `0fba548329afd9bc9dee92ea6caa53d1aaa69eb0`
- Tree:
  `69386cf40e566baad2786a079029a6eea20bd8d1`

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
```

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Checkpoint inspeccionado antes de este reemplazo:
  `59ca0864daac7b79816974679cd4353033fe6408`
- Parent inmediato:
  `179a151d24074e9d4cb8c5f16bdcd6ef49308929`

`atlanticus-cannonical:main` es autoridad documental vigente, subordinada a
`atlanticus:main` cuando la implementación publicada demuestra un estado posterior.

## Referencias históricas

`moragaga/atlanticus-decisions` es **HISTORICAL**.

Puede aportar rationale y evidencia histórica. No puede reemplazar
`atlanticus:main` ni `atlanticus-cannonical:main`.

## Jerarquía

1. `atlanticus:main`: realidad implementada.
2. `atlanticus-cannonical:main`: contratos, fronteras, roadmap y estado vigente.
3. Qualification y tests vigentes: evidencia de propiedades demostradas.
4. Decisiones explícitas del Project todavía no formalizadas en canonical: delta temporal.
5. Referencias históricas indicadas por el usuario.
6. Memoria/historial conversacional: pista, nunca autoridad suficiente.

## Clasificación obligatoria

Distinguir hechos y decisiones con:

```text
VERIFIED
INFERRED
ASSUMED
PROPOSED
UNVERIFIED
```

Y estado con:

```text
CURRENT
IN PROGRESS
PLANNED
SUPERSEDED
BLOCKED
CLOSED
```

Si implementación y canonical se contradicen, exponer el conflicto y actualizar
canonical; nunca retroceder implementación CURRENT para satisfacer documentación
obsoleta.

## Git

Git es **READ ONLY** por defecto.

No crear commits, push, ramas, PR, issues ni mutaciones remotas sin autorización
explícita.

## Continuidad congelada

No reabrir sin conflicto demostrado:

```text
Global Users
standalone / generic

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

Navigation configuration -> Profiles core
ALLOWED / CURRENT

Navigation -> Users
FORBIDDEN

Navigation -> ADA Access
FORBIDDEN

Navigation -> Profiles Configuration
FORBIDDEN

Users login write/pending
FORBIDDEN

OLD SCHEMA RUNTIME READERS
FORBIDDEN

ADAPTERS / SHIMS / ALIASES
FORBIDDEN

DOUBLE CONTRACT
FORBIDDEN
```

Promotion de Users no habilita el ingreso a la aplicación.

El contrato CURRENT de acceso permanece:

```text
authenticated identity + no promoted UserRecord
→ READY
→ deterministic user_id
→ no UsersRuntime user

authenticated identity + promoted enabled UserRecord
→ READY
→ EffectiveUser available in UsersRuntime

authenticated identity + promoted disabled UserRecord
→ USER_DISABLED
→ 403
```

`USER_NOT_PROMOTED` no existe en `AccessStatus` CURRENT.

## Navigation / Profiles CURRENT

Navigation Configuration consume directamente el contrato generic de Profiles core:

```text
ProfileCatalog
ProfileDefinition
```

Contrato de integración:

```text
NavigationProfileCatalogProvider = Callable[[], ProfileCatalog]
```

Sin provider configurado:

```text
profile_definitions() -> ()
Navigation no inventa perfiles base
```

Con provider configurado:

```text
ProfileCatalog.all()
→ perfiles disponibles en la superficie administrativa

ProfileCatalog.require(profile_key)
→ validación referencial de allowed_profiles
```

Fallos del provider no se silencian.

El mismo conjunto de `NavigationProjectionValidator` se usa en validación de draft y
en Projection cuando la composition lo configura.

## Targets superseded

Quedan históricos o reemplazados:

```text
USERS-MANAGER-GENERIC-CONTRACT-CUTOVER
CLOSED / SUPERSEDED

USERS-CLEAN-CUTOVER-COMPLETION
CLOSED / SUPERSEDED

USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL
CLOSED / SUPERSEDED

USERS-STANDALONE-AUTHORITY-CUTOVER
CLOSED / SUPERSEDED BY USERS-GLOBAL-REGISTRY-ROOT-CUTOVER

USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER
SUPERSEDED / NOT EXECUTED AS FINAL TARGET

USERS-PROFILES-COMPOSITION-CUTOVER
SUPERSEDED AS PREVIOUS MODEL

ACCESS-PROFILES-CONFIGURATION as generic Atlanticus Access capability
SUPERSEDED / REJECTED BEFORE INTEGRATION

USER_NOT_PROMOTED -> 403
SUPERSEDED / REMOVED

NavigationProfileOption
SUPERSEDED / REMOVED

NavigationProfileOptionsProvider
SUPERSEDED / REMOVED

_BASE_PROFILES
SUPERSEDED / REMOVED

resolve_profile_options / selectable_profile_options
SUPERSEDED / REMOVED

profile_options_provider
SUPERSEDED / REMOVED
```

## Siguiente foco único recomendado

```text
Manager authorization stale administrator/local semantics
PLANNED / PROPOSED NEXT
```

El nombre de incremento puede fijarse en el siguiente chat; no se considera contrato
CURRENT hasta revisar implementación, canonical y consumers.

No mezclar:

```text
exact guest fallback composition for authenticated non-promoted identities
ADA Access runtime composition
Users Administration UI/repair
Python metadata alignment
WEB-TEST-CONTRACT-CLEANUP
CI/global lint cleanup
```

No inventar contratos nuevos cuando el código CURRENT ya provee una frontera suficiente.
