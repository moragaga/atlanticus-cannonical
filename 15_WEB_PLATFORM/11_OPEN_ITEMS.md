# Web Platform — Open Items

Estado: **OPEN**

Los items cerrados no deben reabrirse para restaurar simetría o legacy.

## Closed baselines relevantes

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-PERSISTED-DATA-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CAPABILITY-EXTRACTION
CLOSED / VERIFIED / CURRENT

PROFILES-INDEPENDENT-SOURCE-LIFECYCLE
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
```

## Configuration UI composition recovery — NEXT

```text
CONFIGURATION-UI-COMPOSITION-RECOVERY
PLANNED / NEXT
```

Open dentro de ese foco de diseño/inventario:

1. inspeccionar Manager shell/home/sidebar/workflow CURRENT;
2. inventariar primitives/composiciones UI reutilizables CURRENT;
3. localizar evidencia histórica concreta de visualizaciones reportadas como perdidas;
4. distinguir comportamiento reusable de estilos/implementación obsoleta;
5. no copiar una composición histórica como autoridad automática;
6. resolver incompatibilidades CURRENT que bloqueen reutilización sin aliases/shims;
7. elegir un único primer módulo UI faltante para el incremento posterior.

## navigation-manager authorization consumer

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

`can_access` debe alinearse al contrato CURRENT `can_view` cuando entre al scope.
No crear compatibility alias.

## Users Administration

```text
USERS-ADMINISTRATION-SURFACE-CUTOVER
PLANNED / SEPARATE
```

Usar `UsersAdministrationService` y contracts actuales.
No reintroducir Users Source/Projection.

## Profiles Configuration UI

```text
PLANNED / SEPARATE INCREMENT
```

Usar `ProfileCatalog`, `ProfilesConfiguration` y Profiles Source lifecycle actuales.
No agregar permisos ADA a Profiles generic.

## ADA Access Configuration UI

```text
PLANNED / SEPARATE INCREMENT
```

Usar `AdaAccessConfiguration` y contracts actuales.
Mantener ownership ADA.

## Navigation runtime fallback

```text
OPEN / SEPARATE
```

No crear Users authority `guest` ni UserRecord ficticio.

## ADA Access runtime

```text
OPEN / SEPARATE
```

No convertirlo en dependency de Navigation.

## Entra / Directory

Provider concreto:

```text
UNVERIFIED
```

No inventar Graph settings/scopes/endpoints.

## Test contract cleanup

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN
```

## Python baseline

```text
PYTHON-METADATA-ALIGNMENT
PLANNED / OPEN
```

## Qualification transversal

```text
full web pytest after final callback delta
UNVERIFIED

full ADA pytest after final callback delta
UNVERIFIED

CI remote
UNVERIFIED

full Ruff workspace
UNVERIFIED
```
