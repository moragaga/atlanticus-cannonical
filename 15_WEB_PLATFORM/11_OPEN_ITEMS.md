# Web Platform — Open Items

Estado: **PLANNED OPEN ITEMS**

Los items cerrados no deben reabrirse para restaurar simetría, legacy o contratos transitorios.

## Closed baselines relevantes

```text
PROFILES-CONFIGURATION-EDITOR-CONTRACT
CLOSED / VERIFIED / CURRENT

PROFILES-MANAGER-COMPOSITION
CLOSED / VERIFIED / CURRENT

USERS-PROFILES-CONTRACT-REALIGNMENT
CLOSED / VERIFIED / CURRENT

USERS-ADMINISTRATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

USERS-MANAGER-UI-REVIEW
CLOSED / CURRENT / ACCEPTED WITH NON-BLOCKING POLISH

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
CLOSED FOR CURRENT V1
```

## Users final automated qualification

```text
CURRENT implementation
moragaga/atlanticus@ce07ada07e3f4f100b97ad2ac5e7285b54419c20

targeted/full Users pytest after final Guest-boundary corrective
UNVERIFIED

Ruff after final corrective
UNVERIFIED
```

Durante el incremento se observó un fallo de tests causado por rechazar `guest` demasiado temprano
en `normalize_managed_profile_key()`. La implementación CURRENT refina esa decisión:

```text
UserRecord(profile_key='guest')
VALID as transient/pending state

administrative assignment guest
FORBIDDEN by require_managed_profile()

available managed assignment options
exclude guest + local
```

No promover la suite final a VERIFIED hasta ejecutar tests sobre el checkpoint CURRENT.

## Residual UI polish

```text
PLANNED / DEFERRED / NON-BLOCKING
```

El cierre del flujo actual acepta que queden detalles visuales menores.

No convertir esos detalles en una reapertura del contrato Users ni del Manager.

No crear tests cuyo único objetivo sea congelar CSS, spacing, colores o estructura visual.

## Manager real persistence qualification

```text
MANAGER-REAL-PERSISTENCE-QUALIFICATION
PLANNED / OPEN
```

Users Administration implementa la secuencia:

```text
promote
UsersRegistryStore.replace
→ UsersAdministrationStore.create

update
UsersRegistryStore.replace
→ UsersAdministrationStore.replace
```

El wiring productivo concreto Blob/Cosmos no quedó calificado en este cierre.

No afirmar persistencia Azure end-to-end hasta verificar composición/configuración productiva.

## Responsive/media queries — PHASE 2

```text
MANAGER-RESPONSIVE-MEDIA-QUERY-AUDIT
PLANNED / DEFERRED
```

Debe revisar shared Manager CSS y estilos de cada capability cuando se retome el frente visual.

## Test cleanup — PHASE 3

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / DEFERRED
```

Eliminar tests de CSS/visual structure/internal classes/functions.

Conservar behavior contracts, invariants, callbacks relevantes, persistencia y recovery.

## Navigation Manager authorization consumer

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

No crear compatibility alias.

## ADA Access runtime

```text
PLANNED / SEPARATE
```

## Navigation runtime/disabled-route work

```text
PLANNED / SEPARATE
```

No mezclar con el siguiente foco de backend KPI.

## Entra / Directory

```text
concrete provider
UNVERIFIED
```

## Python baseline

```text
PYTHON-METADATA-ALIGNMENT
PLANNED / SEPARATE
```

La baseline del Project continúa en Python 3.14.7.

## Qualification transversal

```text
CI remote
UNVERIFIED

full Ruff workspace
UNVERIFIED
```

## Próxima frontera autorizada

El siguiente chat debe tener un único foco:

```text
ADA backend KPI flow
```

Orden conceptual indicado por el Project:

```text
1. KPI Runtime
   permitir ejecución controlada aunque el watermark/current ya haya sido calculado,
   para debug/testing, sin inventar datos ni debilitar durable authority.

2. KPI Delivery + KPI Timeseries Delivery
   verificar y consumir desde Cosmos el contrato de configuración ya materializado por
   la configuración al inicio del flujo.

3. Después de conocer y verificar el final real del flujo
   continuar con el collector en ada-generic.
```

Los puntos 2 y 3 son `PLANNED / UNVERIFIED AGAINST CURRENT MAIN` hasta inspeccionar el código y
contratos vigentes. No modificar implementación basándose sólo en esta descripción.
