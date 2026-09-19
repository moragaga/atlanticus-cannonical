# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

## Autoridad

Implementación publicada CURRENT:

```text
moragaga/atlanticus@fbef06a8a0a587571527d9ecf131c73c5fc5f01a
```

Parent inmediato:

```text
9f12c41a23d69784c7c5b775a4093a94ac654d55
```

Tree:

```text
fc8c293f617aca4a53d89f687a22728e9d0fdcca
```

Canonical inspeccionado para este cierre:

```text
moragaga/atlanticus-cannonical@4e59aa1e5160ff827ca6767fe178d3b45f4bc30d
```

Git permanece SOLO LECTURA para el asistente.

## Estado resumido

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER                  CLOSED / VERIFIED / CURRENT
PROFILES-CONFIGURATION-BOUNDARY-CUTOVER             CLOSED / VERIFIED / CURRENT
PROFILES-CAPABILITY-EXTRACTION                      CLOSED / VERIFIED / CURRENT
PROFILES-INDEPENDENT-SOURCE-LIFECYCLE               CLOSED / VERIFIED / CURRENT
USERS-PERSISTED-DATA-CUTOVER                        CLOSED / VERIFIED / CURRENT
ADA-ACCESS-PROFILES-CONFIGURATION                   CLOSED / VERIFIED / CURRENT
NONPROMOTED-ACCESS-SEMANTICS-CORRECTION             CLOSED / VERIFIED / CURRENT
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT            CLOSED / VERIFIED / CURRENT
MANAGER-AUTHORIZATION-SEMANTICS-ALIGNMENT           CLOSED / VERIFIED / CURRENT
MANAGER-ACTIVE-WORKFLOW-CALLBACK-CARDINALITY        CLOSED / VERIFIED / CURRENT
CONFIGURATION-UI-COMPOSITION-RECOVERY                CLOSED / VERIFIED / CURRENT
GENERIC-WEB-PAGINATION-CUTOVER                       CLOSED / VERIFIED / CURRENT
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT  BLOCKED / VERIFIED CONFLICT
PROFILES-CONFIGURATION-EDITOR-CONTRACT               PLANNED / NEXT
PROFILES-CONFIGURATION-WEB-SURFACE                   PLANNED
USERS-ADMINISTRATION-SURFACE-CUTOVER                 PLANNED / SEPARATE
ADA-ACCESS-CONFIGURATION-UI                          PLANNED / SEPARATE
MANAGER-FINAL-ADMIN-COMPOSITION                      PLANNED / FINAL
ADA-ACCESS-RUNTIME-COMPOSITION                       PLANNED / SEPARATE
WEB-TEST-CONTRACT-CLEANUP                            PLANNED / OPEN
PYTHON-METADATA-ALIGNMENT                            PLANNED / OPEN
```

## VERIFIED

### Published checkpoint

`main` está publicado exactamente en:

```text
fbef06a8a0a587571527d9ecf131c73c5fc5f01a
```

con parent:

```text
9f12c41a23d69784c7c5b775a4093a94ac654d55
```

### Generic Web pagination

El comportamiento transversal de paginación fue extraído desde ADA hacia Atlanticus:

```text
web/framework/core/src/atlanticus/web/pagination.py
```

Contrato CURRENT:

```text
DEFAULT_PAGE_SIZE = 10
ALLOWED_PAGE_SIZES = (10, 20)
PageRequest(page_number=1, page_size=10)
Page(items, total_count, request)
paginate_items(items, request)
```

`PageRequest` valida página positiva y tamaño sólo `10|20`.

`Page` expone:

```text
page_count
has_previous
has_next
start_index
end_index
```

`paginate_items`:

```text
- pagina una colección ya resuelta por el consumer;
- no ordena, filtra ni busca;
- clampa page_number a la última página válida;
- para colección vacía resuelve page_number=1 y conserva page_size;
- devuelve sólo items reales.
```

### ADA pagination legacy removal

Fue removido:

```text
scopes/ada/web/configuration/core/src/ada/web/configuration/pagination.py
```

También se retiraron sus exports y su test específico legacy.

No existen aliases, shims ni reexports para mantener el contrato anterior.

`ada.web.configuration.presentation` permanece ADA-owned y consume:

```text
atlanticus.web.pagination.Page
atlanticus.web.pagination.ALLOWED_PAGE_SIZES
```

La presentación no fue generalizada.

### KPI consumers

KPI Configuration y KPI Definition consumen directamente:

```text
atlanticus.web.pagination.Page
atlanticus.web.pagination.PageRequest
atlanticus.web.pagination.paginate_items
```

`SortDirection` quedó local a KPI Configuration porque la trazabilidad no demostró uso
transversal.

### UI boundary recuperada

Quedó congelada la distinción:

```text
TRANSVERSAL
→ comportamiento de paginación

LOCAL A CADA PRESENTACIÓN
→ markup
→ CSS
→ tabla/cards/modal
→ placeholders visuales
→ responsive
→ acciones de fila
```

Profiles, Users y ADA Access no deben compartir UI sólo por parecerse visualmente.

### Testing policy aplicada al cutover

Se removieron asserts cuyo objetivo era fijar clases/estilos CSS del paginador.

El contrato genérico se prueba por comportamiento.

Qualification observada contra el working tree que luego fue publicado como
`fbef06a8...`:

```text
Atlanticus Web framework/core pytest
53 PASS
Ruff framework/core
PASS

ADA Configuration core pytest
4 PASS
Ruff core
PASS

KPI Configuration pytest
37 PASS
Ruff sobre archivos modificados
PASS

KPI Definition pytest
35 PASS
Ruff sobre archivos modificados
PASS

git diff --check
PASS
```

Total observado:

```text
129 tests PASS
```

Los `test_web_runtime.py` de KPI Configuration y KPI Definition mostraron findings I001 de
orden de imports al ejecutar Ruff sobre paquetes completos antes del cierre. Esos archivos
no fueron modificados por el incremento y quedaron fuera de alcance.

### Manager authorization semantics

Permanece CURRENT:

```text
ManagerModule.access_key: str | None
ManagerAuthorizationPolicy.can_view(principal, module) -> bool
```

No hay bypass por `is_local` ni profile `administrator`.

### UI administrativa CURRENT

ADA Configuration Manager CURRENT compone:

```text
navigation
tools
kpis
kpi-definitions
```

Siguen ausentes:

```text
Profiles Configuration UI
Users Administration UI
ADA Access Configuration UI
```

## VERIFIED CONFLICT

### navigation-manager standalone consumer

En CURRENT:

```text
ManagerAuthorizationPolicy.can_view(...)
```

pero:

```text
web/compositions/navigation-manager/src/.../composition.py
```

invoca:

```text
resolved_authorization.can_access(...)
```

Estado:

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

No fue parte del pagination cutover.

## INFERRED

La extracción confirma una frontera reusable real: la paginación modela estado/cálculo y
puede ser consumida por superficies distintas sin transferir ownership visual.

La similitud de tablas entre módulos no demuestra un componente UI genérico compartido.

Profiles puede consumir `atlanticus.web.pagination` sin depender de ADA.

## ASSUMED

No se asume:

- que Profiles deba copiar markup/CSS de KPI;
- que `ada.web.configuration.presentation` deba moverse a Atlanticus;
- que SortDirection sea genérico;
- que Users/Profiles/ADA Access compartan lifecycle o presentación;
- que full Ruff workspace esté limpio;
- que CI remoto esté verde;
- que metadata Python esté globalmente alineada.

## PROPOSED

Único foco siguiente:

```text
PROFILES-CONFIGURATION-EDITOR-CONTRACT
PLANNED / NEXT
```

Después:

```text
PROFILES-CONFIGURATION-WEB-SURFACE
PLANNED
```

## UNVERIFIED / PENDING

```text
exact guest fallback composition for authenticated non-promoted identities
PLANNED / SEPARATE

Users Administration UI
PLANNED

Profiles Configuration UI
PLANNED

ADA Access Configuration UI
PLANNED

ADA Access runtime composition exacta
PLANNED / SEPARATE

concrete Entra/Graph UsersDirectoryReader provider
UNVERIFIED

full Ruff workspace de fbef06a8...
UNVERIFIED

CI remoto de fbef06a8...
UNVERIFIED

Python metadata global 3.14.7
PLANNED / SEPARATE
```
