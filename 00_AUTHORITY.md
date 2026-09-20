# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Checkpoint CURRENT verificado para este cierre:
  `29bbf6d8f2b47a7d31e967ad4bb8de42f67a4c85`
- Parent inmediato verificado:
  `856498c52f182cd531deae845c25bd51ae2ff4ea`
- Tree verificado:
  `3f27ad599c6dec610dff5317494a73b276d2ebc4`
- Fecha del commit:
  `2026-09-20T04:04:29Z`

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

MANAGER-FINAL-ADMIN-COMPOSITION
CLOSED / VERIFIED / CURRENT

NAVIGATION-STANDALONE-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT

NAVIGATION-CONFIGURATION-UI-PASS
CLOSED / VERIFIED MANUAL / CURRENT

MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS
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
  `deb493659b41c0d8fea5c70674002486b3b92cbc`

`atlanticus-cannonical:main` es autoridad documental vigente, subordinada a
`atlanticus:main` cuando la implementación publicada demuestra un estado posterior.

## Referencias históricas

`moragaga/atlanticus-decisions` es **HISTORICAL**.

Puede aportar rationale y evidencia histórica. No puede reemplazar `atlanticus:main` ni
`atlanticus-cannonical:main`.

No se verificó durante este cierre un decision record histórico que contradiga el contrato
CURRENT de Navigation. Cualquier afirmación adicional sobre `atlanticus-decisions` permanece
UNVERIFIED.

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

Si no existe provider:

```text
Navigation sigue operando de forma autónoma
```

Si existe provider:

```text
la composition externa adapta su catálogo a NavigationProfileOption
la validation de profile keys puede instalarse
```

No existe CURRENT:

```text
Navigation Configuration -> Profiles core dependency
Navigation -> Users dependency
Navigation -> ADA Access dependency
```

En ADA Configuration Manager, la composition adapta Profiles a opciones de Navigation y
excluye `root` y `local` del selector de asignación. Navigation generic no conoce esas keys
como casos especiales.

## Navigation Configuration UI CURRENT

La surface administrativa CURRENT:

- no contiene card separada de profiles;
- asigna profiles únicamente dentro del editor de enlace;
- no autoselecciona `guest`;
- muestra enlace sin profiles como `Acceso: Público`;
- pagina únicamente nodos top-level;
- una sección cuenta como un item top-level;
- hijos de sección no cuentan para el total de página;
- page size: `10 / 20`, default `10`;
- secciones colapsadas inicialmente;
- expansión de sección muestra todos sus hijos;
- múltiples secciones pueden permanecer expandidas;
- el estado expandido es UI efímero y no entra al Source;
- el orden durable global no se redefine por paginación;
- el empty state reserva la superficie de página y se centra;
- el dropdown de page size contiene su focus target interno y no debe producir overflow
  horizontal.

El cierre visual de Navigation fue confirmado manualmente por el usuario sobre el checkpoint
CURRENT.

## Manager UI review CURRENT

```text
MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS
```

El review continúa page-by-page.

Secuencia congelada dentro del mismo foco:

```text
1. presentación desktop/visual de las páginas restantes
2. auditoría responsive/media queries compartidas y capability-locales
3. cleanup/qualification de tests según la frontera vigente
```

No convertir esta secuencia en tres arquitecturas ni mezclar persistencia/runtime.

Existe un cambio CURRENT en:

```text
web/capabilities/manager/src/atlanticus/web/manager/resources/css/10_surface.css
```

donde `.atlanticus-manager__module-page` usa bottom padding `0` tanto en regla base como en
`@media (max-width: 48rem)`.

La corrección visual de esa decisión no está calificada todavía. Debe revisarse en la fase
responsive/media-query; no revertir ni extender a ciegas.

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

El boundary test de Navigation Configuration usa análisis AST de imports; no busca substrings
arbitrarios dentro del source.

## Qualification observada durante el incremento

Antes de los últimos ajustes exclusivamente visuales se observó:

```text
Navigation core
22 passed

Navigation Configuration
42 passed

Navigation Manager
10 passed

ADA Configuration Manager focused
5 passed

Total observado
79 passed
```

También se observó Ruff scoped PASS en Navigation Configuration durante el incremento.

No se recibió en este cierre una ejecución automatizada completa posterior al checkpoint
`29bbf6d8...`.

Por tanto:

```text
post-29bb scoped pytest
UNVERIFIED

post-29bb scoped Ruff
UNVERIFIED

remote CI
UNVERIFIED

full monorepo pytest
UNVERIFIED

full workspace Ruff
UNVERIFIED
```

La validación visual final de Navigation sí quedó confirmada manualmente.

## Siguiente foco único

```text
MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS / NEXT PAGE: HERRAMIENTA
```

No abrir durante ese foco:

```text
Manager real persistence qualification
ADA Access runtime authorization/composition
Navigation operational authorization alignment
Navigation disabled-route surface
concrete Entra/Graph provider
Python metadata alignment
global CI/workspace cleanup
```
