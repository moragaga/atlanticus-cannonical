# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

## Autoridad

Implementación publicada CURRENT:

```text
moragaga/atlanticus@3eb46dac80f23d438774e3afa39999dc96f592d7
```

Parent inmediato:

```text
0fba548329afd9bc9dee92ea6caa53d1aaa69eb0
```

Tree:

```text
69386cf40e566baad2786a079029a6eea20bd8d1
```

Canonical inspeccionado para este cierre:

```text
moragaga/atlanticus-cannonical@59ca0864daac7b79816974679cd4353033fe6408
```

Git permanece SOLO LECTURA para el asistente.

## Estado resumido

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER               CLOSED / VERIFIED / CURRENT
PROFILES-CONFIGURATION-BOUNDARY-CUTOVER          CLOSED / VERIFIED / CURRENT
PROFILES-CAPABILITY-EXTRACTION                   CLOSED / VERIFIED / CURRENT
PROFILES-INDEPENDENT-SOURCE-LIFECYCLE            CLOSED / VERIFIED / CURRENT
USERS-PERSISTED-DATA-CUTOVER                     CLOSED / VERIFIED / CURRENT
ADA-ACCESS-PROFILES-CONFIGURATION                CLOSED / VERIFIED / CURRENT
NONPROMOTED-ACCESS-SEMANTICS-CORRECTION          CLOSED / VERIFIED / CURRENT
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT         CLOSED / VERIFIED / CURRENT
USERS-ADMINISTRATION-SURFACE-CUTOVER             PLANNED / SEPARATE
MANAGER AUTHORIZATION STALE SEMANTICS            PLANNED / PROPOSED NEXT
WEB-TEST-CONTRACT-CLEANUP                        PLANNED / OPEN
PYTHON-METADATA-ALIGNMENT                        PLANNED / OPEN
```

Los hitos genéricos de Manager, Navigation, Tools, KPI Configuration,
KPI Definition y ADA Configuration Manager cerrados anteriormente permanecen
`CLOSED / VERIFIED / CURRENT`.

## VERIFIED

### Published checkpoint

`main` está publicado exactamente en:

```text
3eb46dac80f23d438774e3afa39999dc96f592d7
```

con parent inmediato:

```text
0fba548329afd9bc9dee92ea6caa53d1aaa69eb0
```

El commit contiene únicamente el cutover Navigation ↔ Profiles y su qualification
asociada; Git no fue mutado por el asistente.

### Navigation / Profiles dependency alignment

El mini-modelo local de Navigation Configuration fue removido.

Ya no existen como contrato CURRENT:

```text
NavigationProfileOption
NavigationProfileOptionsProvider
_BASE_PROFILES
resolve_profile_options
selectable_profile_options
profile_options_provider
```

Navigation Configuration declara dependencia directa:

```text
atlanticus-web-profiles==0.1.0
```

sin dependencia a:

```text
atlanticus-web-profiles-configuration
atlanticus-web-users
ADA
```

Contrato CURRENT:

```text
NavigationProfileCatalogProvider = Callable[[], ProfileCatalog]
```

Sin provider:

```text
profile_definitions() == ()
```

Con provider:

```text
profile_definitions(provider) == provider().all()
```

Fallos del provider se propagan; no se convierten en catálogo vacío.

### Referential validation

`create_navigation_profile_catalog_validator(...)` valida cada key configurada en
Navigation mediante:

```text
ProfileCatalog.require(profile_key)
```

Perfil desconocido produce:

```text
code = navigation.profile.unknown
```

Fallos del provider se propagan.

La composición Navigation Manager construye una sola colección `validators` y la usa en:

```text
NavigationManagerDraftValidationWorkflow
NavigationProjectionBuilder / projection service
```

Warnings no invalidan draft; issues `error` sí lo invalidan.

### Durable Navigation contract

No cambió el dominio durable:

```text
NavigationLinkConfiguration.allowed_profiles
→ tuple[str, ...] de profile keys
```

Navigation no persiste copias de `ProfileDefinition`.

Navigation core conserva autorización:

```text
principal.unrestricted
OR
principal.access_key ∈ allowed_profiles
```

### Web administration

La superficie administrativa toma perfiles directamente del `ProfileCatalog` provisto
por composition.

Sin catálogo configurado muestra ausencia de catálogo; no inventa `local`,
`administrator` ni `guest`.

Los badges usan `ProfileDefinition.label/background_color/text_color` y ya no contienen
semántica CSS `unrestricted`.

### Qualification observada

Entorno local reportado:

```text
Python 3.14.7
uv
```

Resultados observados:

```text
uv lock --check
PASS

pytest
capabilities/navigation/configuration/tests
compositions/navigation-manager/tests
PASS / 100%

pytest
capabilities/profiles/core/tests
capabilities/navigation/core/tests
capabilities/navigation/configuration/tests
compositions/navigation-manager/tests
PASS / 100%

ruff check
4 archivos modificados de navigation-manager
PASS

ruff format --check
4 archivos modificados de navigation-manager
PASS

git diff --check
PASS

rg legacy navigation profile symbols
0 matches
```

## INFERRED

La eliminación de `_BASE_PROFILES` y el consumo directo de `ProfileCatalog` demuestra
que Navigation Configuration dejó de poseer un catálogo paralelo de Profiles.

La ausencia de provider no convierte Profiles en dependencia obligatoria de runtime:
Navigation conserva keys durables y el core de autorización permanece desacoplado del
lifecycle/storage de Profiles.

El mismo validator referencial en draft y projection evita reglas divergentes entre
ambas rutas dentro de Navigation Manager.

## ASSUMED

No se asume en este cierre:

- wiring runtime exacto `root/local -> NavigationPrincipal.unrestricted` en todas las compositions;
- fallback exacto `guest` para identidad autenticada sin promoted `UserRecord`;
- que ADA Access runtime deba alimentar directamente Navigation;
- Users Administration UI;
- concrete Entra/Graph `UsersDirectoryReader` provider;
- full Ruff workspace limpio;
- CI remoto PASS;
- metadata global Python alineada a 3.14.7.

## PROPOSED

Único foco recomendado para el siguiente chat:

```text
Manager authorization stale administrator/local semantics
PLANNED / PROPOSED NEXT
```

Razón: implementación CURRENT aún contiene bypass explícito de Manager mediante
`principal.is_local` y `administrator` profile, y ADA Configuration Manager mantiene
helpers equivalentes. El siguiente chat debe debatir ese contrato antes de editar.

## SUPERSEDED / REFINED

### Navigation local profile model

```text
NavigationProfileOption
_BASE_PROFILES
local/administrator/guest definidos dentro de Navigation Configuration
```

queda:

```text
SUPERSEDED / REMOVED
```

### `administrator` como profile especial de Navigation Configuration

Queda removido del catálogo/configuration boundary.

No se introduce mapping:

```text
administrator -> root
```

### `local` como ProfileDefinition especial de Navigation Configuration

Queda removido.

`local` continúa siendo concern de runtime/composition donde corresponda; no es un
`ProfileDefinition` inventado por Navigation Configuration.

### Provider anterior

```text
NavigationProfileOptionsProvider
profile_options_provider
```

queda reemplazado por:

```text
NavigationProfileCatalogProvider
profile_catalog_provider
```

sin shim ni alias.

### Validator naming

El parámetro de composición `projection_validators` quedó reemplazado por `validators`
porque el mismo contrato aplica a draft y projection.

No se conserva alias legacy.

## UNVERIFIED / OPEN

### Manager authorization stale semantics

CURRENT todavía debe revisar:

```text
principal.is_local -> Manager full access
'administrator' in principal.profile_keys -> Manager full access
```

y helpers equivalentes en ADA Configuration Manager.

Estado:

```text
OPEN / SEPARATE / PROPOSED NEXT
```

### Non-promoted Navigation fallback

La entrada de identidades autenticadas no promovidas está cerrada a nivel Identity/Users,
pero la materialización exacta del `NavigationPrincipal`/perfil de fallback sigue fuera de
este hito.

Estado:

```text
OPEN / SEPARATE
```

No resolver creando `guest` authority en Users ni un `UserRecord` ficticio.

### Users Administration

```text
USERS-ADMINISTRATION-SURFACE-CUTOVER
PLANNED / SEPARATE
```

### ADA Access runtime composition

No se modificó en este hito.

```text
OPEN / SEPARATE
```

### Web test contract cleanup

Ruff sobre los directorios completos encontró formato/lint preexistente en:

```text
capabilities/navigation/configuration/tests/test_web_contract.py
capabilities/navigation/configuration/tests/test_web_source_contract.py
```

No fueron modificados oportunistamente.

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN
```

### Python metadata

Baseline del Project:

```text
Python 3.14.7
python:3.14.7-slim-trixie
```

El package Navigation Configuration CURRENT todavía declara:

```text
requires-python = "==3.14.2"
```

```text
PYTHON-METADATA-ALIGNMENT
PLANNED / OPEN
```

### CI / full lint

No se verificó CI remoto ni full Ruff workspace para `3eb46dac...`.

```text
UNVERIFIED
```

## Conflictos canonical detectados antes de este reemplazo

Canonical `59ca0864...` todavía afirmaba:

```text
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
PLANNED / NEXT

NavigationProfileOption / _BASE_PROFILES
CURRENT

Navigation integration
pending
```

Implementación CURRENT demuestra:

```text
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
CLOSED / VERIFIED / CURRENT

NavigationProfileOption / _BASE_PROFILES
REMOVED

Navigation Configuration -> Profiles core
CURRENT
```

Además, documentos canonical anteriores mantenían estados históricos ya superados de
Users/Profiles/Persisted Data; estos reemplazos deben reflejar los cierres ya recogidos
en `00_AUTHORITY.md` y `15_WEB_PLATFORM/12_USERS_PROFILES_NAVIGATION_CAPABILITY_BOUNDARY.md`.

## Siguiente frontera

```text
Manager authorization stale administrator/local semantics
PLANNED / PROPOSED NEXT
```

No mezclar:

```text
Navigation guest fallback runtime composition
ADA Access runtime composition
Users Administration UI/repair
Python metadata
Web test cleanup
Command Center
unrelated Ruff cleanup
```
