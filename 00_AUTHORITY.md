# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Checkpoint CURRENT verificado para este cierre:
  `df5b99502265758e873e0565abf2176cc617104b`
- Parent inmediato verificado:
  `31723a108ddd2f49346fdcbb844db9891eb08f4b`
- Tree verificado:
  `de1151ba72d44bc8ac6b6f2cfd6f57eb7e80c0a0`
- Fecha del commit:
  `2026-09-20T18:26:04Z`

Estado acumulado relevante:

```text
PROFILES-CONFIGURATION-EDITOR-CONTRACT
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-WEB-SURFACE
CLOSED / VERIFIED / CURRENT

PROFILES-PROJECTION-CONTRACT
CLOSED / VERIFIED / CURRENT

PROFILES-MANAGER-COMPOSITION
CLOSED / VERIFIED / CURRENT

PROFILES-MANAGER-UI-REVIEW
CLOSED / VERIFIED MANUAL / CURRENT

USERS-PROFILES-CONTRACT-REALIGNMENT
CLOSED / VERIFIED / CURRENT

USERS-ADMINISTRATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROFILE-OWNERSHIP-REALIGNMENT
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROJECTION-PERSISTENCE
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

ACCESS-UNRESTRICTED-PROFILES-CONTRACT
CLOSED / VERIFIED / CURRENT

ACCESS-MANAGER-UI-REVIEW
CLOSED / VERIFIED MANUAL / CURRENT

MANAGER-FINAL-ADMIN-COMPOSITION
CLOSED / VERIFIED / CURRENT

NAVIGATION-STANDALONE-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT

NAVIGATION-CONFIGURATION-UI-PASS
CLOSED / VERIFIED MANUAL / CURRENT

MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS / NEXT PAGE: USERS
```

Permanece un conflicto implementado previo y separado:

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

`web/compositions/navigation-manager` usa `authorization.can_access(...)`, mientras el contrato
CURRENT de `ManagerAuthorizationPolicy` expone `can_view(...)`.

No resolver mediante alias, shim ni doble contrato.

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Checkpoint inspeccionado antes de este reemplazo:
  `07a0582c7acdd5c9b93f2a1bb02651e8c5302448`

`atlanticus-cannonical:main` es autoridad documental vigente, subordinada a
`atlanticus:main` cuando la implementación publicada demuestra un estado posterior.

## Referencias históricas

`moragaga/atlanticus-decisions` es **HISTORICAL**.

Puede aportar rationale y evidencia histórica. No puede reemplazar `atlanticus:main` ni
`atlanticus-cannonical:main`.

No se inspeccionó `atlanticus-decisions` durante este cierre. Por tanto, cualquier conflicto
nuevo con decisiones históricas permanece **UNVERIFIED**.

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

## Profiles CURRENT

Profiles continúa siendo capability generic Atlanticus.

Contrato de dominio congelado:

```text
ProfileDefinition
ProfileCatalog
ProfilesConfiguration
```

System profiles CURRENT:

```text
basic
root
guest
local
```

El incremento de cierre visual no modifica ese dominio ni introduce estado durable adicional.

La surface administrativa CURRENT:

- muestra `Fuente de verdad` y `Proyección` mediante nombres inyectados por composition;
- conserva defaults generic `Profiles Source` / `Profiles Projection`;
- los providers generic declaran `Local Source` / `Local Projection` y
  `Blob Storage` / `Cosmos DB` según la topología real;
- ADA local compone `Perfiles`, la descripción visible del módulo, `Local Source` e
  `In-process Projection`;
- los perfiles configurados usan `atlanticus.web.pagination`, page sizes `10 / 20`, botones de
  página y resumen `Mostrando X–Y de Z`;
- el editor usa un modal capability-local centrado respecto del viewport, con backdrop;
- el editor muestra preview del nombre, color de fondo, color de texto y valores hex actuales;
- un perfil normal usa una única inicial mayúscula como avatar visual;
- `local` es el único caso visual especial: no se representa como un único color fijo, sino a
  través de las identidades locales disponibles;
- una identidad local usa inicial del primer nombre + inicial del último nombre en mayúsculas;
- `Jane Doe` y `John Doe` pueden mostrar ambos `JD`; sus colores distinguen las identidades;
- el footer no reserva espacio vacío cuando no existe resultado.

El usuario confirmó manualmente el resultado final sobre la implementación publicada
`df5b995...` y declaró Perfiles cerrado.

## Navigation CURRENT

Navigation es generic Atlanticus y debe poder componerse sin Profiles.

Contrato durable:

```text
NavigationLinkConfiguration.allowed_profiles
```

Semántica CURRENT:

```text
enabled = False
→ inaccesible

enabled = True + allowed_profiles = ()
→ público dentro de la autorización Navigation

enabled = True + allowed_profiles no vacío
→ restringido a esas profile keys

principal.unrestricted = True
→ bypass de restricción de profiles, no de disabled
```

`Navigation Configuration` ya no depende físicamente de Profiles.

Contrato neutral CURRENT:

```text
NavigationProfileOption(key, label)
NavigationProfileOptionsProvider
```

El provider es opcional.

En ADA Configuration Manager, la composition adapta Profiles a opciones de Navigation y
excluye `root` y `local` del selector de asignación. Navigation generic no conoce esas keys
como casos especiales.

## ADA Access CURRENT

Contrato durable CURRENT:

```text
AdaAccessConfiguration
├── access_keys: tuple[str, ...]
└── profile_access: tuple[ProfileAccessGrant, ...]
```

Semántica de profiles CURRENT:

```text
root / local
→ todos los access_keys definidos
→ explicit grants forbidden

basic / guest / custom profiles
→ grants explícitos configurables
```

La UI Manager de Access permanece cerrada y visualmente aceptada.

## Manager UI review CURRENT

```text
MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS
```

Slices cerrados:

```text
Navigation
CLOSED / VERIFIED MANUAL / CURRENT

Accesos
CLOSED / VERIFIED MANUAL / CURRENT

Perfiles
CLOSED / VERIFIED MANUAL / CURRENT
```

Siguiente foco único:

```text
Users
NEXT
```

`Users` continúa siendo `ManagerEntry`, no `ManagerModule`; no inventar Source/Projection para
hacerlo parecerse a las páginas de configuración.

`Herramienta` permanece **OPEN / DEFERRED** y no queda implícitamente calificada por el cierre
de Perfiles ni por el futuro cierre de Users.

El usuario indicó que, después de Users, desea cerrar el trabajo de Manager **por ahora**.
Eso debe interpretarse como un punto de cierre del alcance actual, no como qualification
implícita de frentes diferidos.

## Testing Web CURRENT

Automatizar comportamiento y contratos.

No congelar con tests:

```text
CSS visual
spacing
colores
tamaños
responsive visual
overflow visual
forma visual de paginación
estructura visual accidental
contenido/estructura interna de JS
existencia/no existencia de clases internas
existencia/no existencia de funciones internas
nombres privados
```

Assets JS/CSS sólo pueden comprobarse como presentes/cargables cuando su carga sea un
requisito contractual real.

Paginación sí puede probarse como comportamiento funcional.

## Qualification de este cierre

VERIFIED:

```text
atlanticus:main = df5b99502265758e873e0565abf2176cc617104b
parent = 31723a108ddd2f49346fdcbb844db9891eb08f4b
tree = de1151ba72d44bc8ac6b6f2cfd6f57eb7e80c0a0
Profiles final visual review = accepted manually by user
```

UNVERIFIED para el checkpoint final:

```text
post-df5b targeted pytest execution
post-df5b targeted Ruff execution
remote CI
full monorepo pytest
full workspace Ruff
```

No convertir ausencia de evidencia automatizada en un PASS implícito.

## Siguiente foco único

```text
USERS-MANAGER-UI-REVIEW
PLANNED / NEXT
```

No abrir durante ese foco:

```text
Herramienta en paralelo
Manager real persistence qualification
ADA Access runtime authorization/composition
Navigation operational authorization alignment
Navigation disabled-route surface
concrete Entra/Graph provider
Python metadata alignment
global CI/workspace cleanup
```
