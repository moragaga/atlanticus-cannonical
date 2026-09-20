# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Checkpoint CURRENT verificado para este cierre:
  `31723a108ddd2f49346fdcbb844db9891eb08f4b`
- Parent inmediato verificado:
  `29bbf6d8f2b47a7d31e967ad4bb8de42f67a4c85`
- Tree verificado:
  `4213a0dd11abbc9cb22fb02ed4a60d08bf0f87c5`
- Fecha del commit:
  `2026-09-20T17:27:18Z`

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
  `a48ae6d1433b5ae39288d3b41002782efa10c9cd`

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

El cierre visual de Navigation fue confirmado manualmente por el usuario.

## ADA Access CURRENT

Contrato durable CURRENT:

```text
AdaAccessConfiguration
├── access_keys: tuple[str, ...]
└── profile_access: tuple[ProfileAccessGrant, ...]
```

`access_key` sigue siendo una identidad estable única. La UI CURRENT crea nuevas keys mediante:

```text
ámbito + permiso
→ ámbito.permiso
```

No existe una entidad durable separada `scope`, `grant`, `label` o `display_name`.

Semántica de profiles CURRENT:

```text
root
→ todos los access_keys definidos
→ explicit grants forbidden

local
→ todos los access_keys definidos
→ explicit grants forbidden

basic / guest / custom profiles
→ grants explícitos configurables
```

`remove_access_key(...)` continúa rechazando eliminar una key todavía asignada.

La validación de draft de Access continúa dependiendo de una Profiles Projection activa.
La UI, cuando no existe Profiles Projection activa, puede mostrar el catálogo de sistema
proporcionado por `ProfileCatalog()`; al excluir `root` y `local`, permanecen `basic` y `guest`
como perfiles asignables.

## ADA Access Manager UI CURRENT

La surface administrativa CURRENT:

- usa tabs secundarios `Accesos` y `Perfiles`;
- pagina ambos listados mediante `atlanticus.web.pagination`;
- page sizes `10 / 20`, default `10`;
- conserva la reserva vertical de página coherente con las surfaces ya calificadas;
- corrige la causa del overflow horizontal del dropdown sin ocultarlo globalmente;
- usa el mismo lenguaje visual de Source/Projection ya validado en Navigation;
- muestra cada perfil como fila estable con resumen `N accesos` y acción `Configurar`;
- no usa multiselect inline creciente;
- `Configurar` queda deshabilitado cuando no existen accesos definidos;
- el modal de asignación se centra respecto del viewport;
- el modal usa `dbc.Checkbox`, no `dcc.Checklist`;
- las asignaciones confirmadas modifican directamente la configuración editable;
- no existe un segundo contrato durable ni un overlay persistente de grants;
- el footer `Borrador local · accesos` conserva la alineación de las demás surfaces;
- la presentación mobile de filas de perfiles quedó revisada y aceptada manualmente.

En ADA Configuration Manager, el título visible de la capability generic Profiles se compone
como `Perfiles`; el default generic de `compose_profiles_manager` no fue cambiado.

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
```

El siguiente foco acordado por el usuario es:

```text
Perfiles
NEXT
```

`Herramienta` continúa pendiente dentro del review. El cambio de orden no la cierra ni la
supersede.

Secuencia congelada dentro del mismo foco:

```text
1. presentación desktop/visual page-by-page
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

La corrección visual global de esa decisión todavía no está calificada como fase transversal.
Debe revisarse en la fase responsive/media-query; no revertir ni extender a ciegas.

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

## Qualification observada durante el hito Access

Evidencia explícita observada antes de la última corrección mínima:

```text
ADA Access Configuration
46 passed + 1 failing test
```

El único fallo era el uso de `dash.html.Input`, no disponible en el runtime instalado.
Fue reemplazado por `dbc.Checkbox`.

El usuario confirmó después que la corrección final quedó completamente OK antes de publicar
el checkpoint `31723a1...`.

También se observó:

```text
ADA Configuration Manager
31 passed

git diff --check
PASS
```

El Ruff completo de `ada-configuration-manager` reportó tres `I001` en archivos no modificados
por este hito:

```text
kpi_definitions.py
kpis.py
workflows.py
```

No fueron corregidos porque estaban fuera de alcance.

Permanece:

```text
remote CI
UNVERIFIED

full monorepo pytest
UNVERIFIED

full workspace Ruff
UNVERIFIED
```

## Siguiente foco único

```text
MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS / NEXT PAGE: PERFILES
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
