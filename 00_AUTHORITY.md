# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Checkpoint CURRENT verificado para este cierre:
  `6032cf84e8a5ad1f7a4cde4333513a04bcdd659a`
- Parent inmediato verificado:
  `783d3578da52aeb5cf831999a7717dc8b79f2fb0`

El checkpoint CURRENT está exactamente un commit por delante de `783d3578...` y contiene el
incremento ADA Access Configuration Manager junto con el fix de compatibilidad de Users
Administration para `dash-bootstrap-components==2.0.4`.

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

USERS-MANAGER-CHECKLIST-COMPATIBILITY
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROFILE-OWNERSHIP-REALIGNMENT
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROJECTION-CONTRACT
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROJECTION-PERSISTENCE
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

MANAGER-FINAL-ADMIN-COMPOSITION
CLOSED / VERIFIED / CURRENT

MANAGER-ALL-SURFACES-RENDERABLE
CLOSED / VERIFIED MANUAL / CURRENT
```

Permanece un conflicto implementado previo:

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

`web/compositions/navigation-manager` no debe tratarse como alineado hasta que consuma
directamente el contrato CURRENT de autorización Manager. No crear alias/shim para preservar
ese consumer.

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Checkpoint inspeccionado antes de este reemplazo:
  `701bbad0486540c8a6c067df37fc70d6f13a8d4e`

`atlanticus-cannonical:main` es autoridad documental vigente, subordinada a
`atlanticus:main` cuando la implementación publicada demuestra un estado posterior.

## Referencias históricas

`moragaga/atlanticus-decisions` es **HISTORICAL**.

Puede aportar rationale y evidencia histórica. No puede reemplazar `atlanticus:main` ni
`atlanticus-cannonical:main`.

No se verificó durante este cierre un decision record histórico que contradiga el contrato
CURRENT. Cualquier afirmación adicional sobre `atlanticus-decisions` permanece UNVERIFIED.

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

ADA Access legacy Source schema readers
FORBIDDEN

ADA Access legacy Projection schema readers
FORBIDDEN

AccessDefinition wrapper/entity without independent behavior
NOT REQUIRED / SUPERSEDED
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
ManagerEntry.access_key: str | None
ManagerAuthorizationPolicy.can_view(principal, item)
```

No conceden autoridad implícita:

```text
principal.is_local
administrator profile
```

El mismo permiso funcional del item protege visibilidad y operaciones administrativas.

## Users / Profiles CURRENT

Profiles posee definición y catálogo de perfiles.

Users posee:

```text
identity + lifecycle + user -> profile_key
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

Users Manager:

```text
web/compositions/users-manager
ManagerEntry
users.manage
/manager/users
```

El fix CURRENT de Users Administration no cambia contratos de dominio. Corrige únicamente
la construcción del control `dbc.Checklist` para dbc 2.0.4, moviendo `disabled` a la opción
del Checklist.

## Profiles CURRENT

```text
profiles/core
profiles/configuration
profiles/projection-local
profiles/projection-cosmos
web/compositions/profiles-manager
```

Profiles Projection materializa `ProfileCatalog`.

La Web surface de Profiles y la composition Manager reusable están implementadas.

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
AccessDefinition wrapper sin responsabilidad independiente
```

Contrato CURRENT:

```text
ProfileAccessGrant(profile_key, access_keys)
EffectiveAdaAccess(profile_key, access_keys)

AdaAccessConfiguration
├── access_keys: tuple[str, ...]
└── profile_access: tuple[ProfileAccessGrant, ...]
```

`access_key` es la identidad estable del acceso. No existe UUID paralelo, `access_id` ni
`permission_id`.

Invariantes CURRENT:

- access keys normalizadas y únicas;
- grants únicos por profile;
- un grant sólo puede referenciar access keys declaradas;
- una access key asignada no puede eliminarse hasta retirar sus asignaciones;
- profile keys se validan contra `ProfileCatalog`;
- no se persiste `access_keys` dentro de Users;
- no existe compatibilidad runtime con schemas anteriores.

Source CURRENT:

```text
ADA_ACCESS_SOURCE_SCHEMA_VERSION = 3
access/configuration.json.gz
```

Projection durable CURRENT:

```text
ADA_ACCESS_PROJECTION_SCHEMA_VERSION = 2
ProjectionRecord[AdaAccessConfiguration]
exact recursive dependencies
local provider
Cosmos provider
```

Dependencia exacta:

```text
Profiles ProjectionTarget
        ↓
ADA Access ProjectionTarget
```

Web/Manager CURRENT:

```text
scopes/ada/web/access/configuration/.../web
ManagerModule key = access
route = /access
effective Manager route = /manager/access
access_key = access.manage
```

La UI permite definir/eliminar access keys y asignarlas a Profiles provenientes de la
Profiles Projection.

La integración manual del desarrollador sigue siendo el modelo previsto: Access no descubre
funcionalidades ni modifica consumidores automáticamente.

## ADA Configuration Manager CURRENT

Composición CURRENT:

```text
Administración
└── Users

Configuraciones
├── Profiles
├── Accesos
├── Navegación
├── Herramienta
├── KPI
└── Definiciones KPI
```

Las superficies fueron observadas manualmente como renderizables en el cierre.

Esto no equivale a qualification visual completa ni a qualification real de persistencia.

## Qualification automatizada del incremento

Evidencia ejecutada durante el cierre:

```text
ADA Access Configuration
37 passed
Ruff scoped PASS
Ruff format scoped PASS

ADA Access Projection Local
4 passed

ADA Access Projection Cosmos
6 passed

ADA Configuration Manager
31 passed
Ruff scoped PASS
Ruff format scoped PASS
```

Remote CI, full monorepo pytest y full global Ruff no fueron verificados en este cierre.

## Testing Web CURRENT

Automatizar comportamiento y contratos.

No congelar con tests:

```text
CSS visual
spacing
colores
tamaños
estructura visual accidental
contenido/estructura interna de JS
existencia/no existencia de clases internas
existencia/no existencia de funciones internas
nombres privados
```

Assets JS/CSS sólo pueden comprobarse como presentes/cargables cuando su carga sea un
requisito contractual real.

Durante el siguiente review de UI, cualquier test existente cuyo único objetivo viole esta
frontera debe eliminarse, no adaptarse para congelar otra estructura visual.

Paginación puede probarse cuando se valida comportamiento funcional; no se testea su forma
visual.

## Estado de qualification pendiente

```text
MANAGER-UI-CONSISTENCY-REVIEW
PLANNED / NEXT

WEB-TEST-CONTRACT-CLEANUP
PLANNED / NEXT / COUPLED TO UI REVIEW

MANAGER-REAL-PERSISTENCE-QUALIFICATION
PLANNED / AFTER UI REVIEW
```

La siguiente qualification funcional real debe comprobar después del cleanup visual:

```text
edit
save draft
validate
publish Source
project
reload
persistence
conflicts/retry where applicable
```

No declarar esas propiedades VERIFIED antes de ejecutarlas.

## Otros frentes separados

No mezclar con el siguiente incremento:

```text
ADA Access runtime authorization/composition
Navigation operational authorization alignment
Navigation disabled-route surface
concrete Entra/Graph provider
Python metadata alignment
global CI/workspace cleanup
```

## Siguiente foco único recomendado

```text
MANAGER-UI-CONSISTENCY-REVIEW
PLANNED / NEXT
```

Alcance: revisar todas las superficies Manager ya visibles, corregir problemas de UI y
paginación, y retirar tests visuales/estructurales inválidos encontrados durante el proceso.

No probar todavía persistencia real ni abrir runtime authorization.
