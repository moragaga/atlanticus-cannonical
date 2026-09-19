# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

## Autoridad

Implementación publicada CURRENT:

```text
moragaga/atlanticus@a31fce11d26a7c0a554d82de1813a4311522919b
```

Parent inmediato:

```text
90e89c376dfdfd182f0380b1d407127ecb7c9711
```

Tree:

```text
737310de59774f3033607c1ef17c1c921efe1e09
```

Canonical inspeccionado para este cierre:

```text
moragaga/atlanticus-cannonical@a7adef2568d664ee31cb1b0eb1fe9f11ce2b9203
```

Git permanece SOLO LECTURA para el asistente.

## Estado resumido

```text
PROFILES-CONFIGURATION-EDITOR-CONTRACT               CLOSED / VERIFIED / CURRENT
PROFILES-CONFIGURATION-WEB-SURFACE                   CLOSED / VERIFIED / CURRENT
PROFILES-PROJECTION-CONTRACT                         CLOSED / VERIFIED / CURRENT
PROFILES-MANAGER-COMPOSITION                         CLOSED / VERIFIED / CURRENT
USERS-PROFILES-CONTRACT-REALIGNMENT                  CLOSED / VERIFIED / CURRENT
ADA-ACCESS-PROFILE-OWNERSHIP-REALIGNMENT             CLOSED / VERIFIED / CURRENT
ADA-ACCESS-PROJECTION-CONTRACT                       CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROJECTION-PERSISTENCE                    PLANNED / NEXT / DESIGN FIRST

USERS-ADMINISTRATION-SURFACE-CUTOVER                 PLANNED / SEPARATE
ADA-ACCESS-CONFIGURATION-UI                          PLANNED / SEPARATE
MANAGER-FINAL-ADMIN-COMPOSITION                      PLANNED / SEPARATE
ADA-ACCESS-RUNTIME-COMPOSITION                       PLANNED / SEPARATE

NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT  BLOCKED / VERIFIED CONFLICT
WEB-TEST-CONTRACT-CLEANUP                            PLANNED / OPEN
PYTHON-METADATA-ALIGNMENT                            PLANNED / OPEN
```

## VERIFIED

### Published checkpoint

`main` está publicado exactamente en:

```text
a31fce11d26a7c0a554d82de1813a4311522919b
```

### Profiles administrative capability

CURRENT:

```text
profiles/core
profiles/configuration
profiles/projection-local
profiles/projection-cosmos
web/compositions/profiles-manager
```

Contratos congelados:

```text
ProfileCatalog
ProfilesConfiguration
Profiles Source lifecycle
Profiles Projection -> ProfileCatalog
```

La UI de Profiles es Profiles-owned. La composition reusable `profiles-manager` existe.
La integración final de todas las superficies en una única aplicación administrativa
permanece separada.

### Users -> Profiles realignment

CURRENT:

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

Users consume `ProfileCatalog` para validar perfiles de managed users.

Managed users no pueden usar `local`.

`local` se conserva para runtime local.

Persistencia CURRENT:

```text
Blob Users Registry schema 2
Cosmos Users schema 2
Users session snapshot v4
```

No hay old-schema runtime readers.

Qualification observada antes de publicar el cutover:

```text
Users core/blob/cosmos pytest
49 PASS

Ruff
PASS

git diff --check
PASS
```

### ADA Access ownership realignment

CURRENT ownership:

```text
profile_key -> access_keys
```

REMOVED:

```text
UserProfileAssignment
AdaAccessConfiguration.user_profiles
user_id -> profile_keys
EffectiveAdaAccess.user_id
EffectiveAdaAccess.profile_keys
```

Source schema CURRENT:

```text
ADA_ACCESS_SOURCE_SCHEMA_VERSION = 2
```

Qualification observada antes de publicar:

```text
Access core pytest
5 PASS

Access configuration pytest
13 PASS

Ruff
PASS

git diff --check
PASS
```

### ADA Access Projection contract

CURRENT:

```text
AdaAccessProjectionBuilder
create_ada_access_projection_service(...)
AdaAccessConfigurationProjectionError
```

Payload:

```text
ProjectionRecord[AdaAccessConfiguration]
```

Dependencia:

```text
ADA Access ProjectionTarget
└── exact Profiles ProjectionTarget
```

La Projection valida `profile_key` contra el `ProfileCatalog` de la Projection de Profiles.

Si Profiles cambia entre target selection y execution, la proyección falla.

Qualification observada antes de publicar:

```text
Access configuration pytest
20 PASS

Ruff
PASS

git diff --check
PASS
```

## INFERRED

Un store durable de ADA Access Projection debe preservar el `ProjectionRecord` completo,
incluyendo `dependencies`, porque `ProjectionRecord.target` incorpora esas dependencias.

El serializer de Profiles no puede copiarse mecánicamente para ADA Access: Profiles no
tiene dependencia upstream en su record actual, ADA Access sí.

Esta inferencia debe verificarse contra los contracts genéricos y providers existentes antes
de implementar el siguiente incremento.

## ASSUMED

No se asume:

- nombre físico de packages nuevos de ADA Access Projection;
- forma final del documento durable;
- topology Cosmos exacta;
- que Profiles serializer sea reusable sin cambios;
- que Manager final ya integre Profiles/Users/ADA Access;
- que ADA Access runtime composition esté resuelta;
- que CI remoto o full Ruff workspace estén verdes;
- que metadata Python esté globalmente alineada.

## PROPOSED

Único foco siguiente:

```text
ADA-ACCESS-PROJECTION-PERSISTENCE
PLANNED / NEXT / DESIGN FIRST
```

La etapa inicial debe inspeccionar primero:

```text
ProjectionStore
ProjectionRecord / ProjectionTarget
Profiles projection-local
Profiles projection-cosmos
Profiles projection serializer
ADA Access current source_projection
```

y sólo después proponer contrato durable/providers.

## UNVERIFIED / PENDING

```text
exact package placement for ADA Access Projection providers
UNVERIFIED

exact durable document contract
UNVERIFIED

exact Cosmos storage topology
UNVERIFIED

Users Administration UI
PLANNED

ADA Access Configuration UI
PLANNED

Manager final administrative composition
PLANNED

ADA Access runtime composition
PLANNED / SEPARATE

concrete Entra/Graph UsersDirectoryReader provider
UNVERIFIED

full Ruff workspace
UNVERIFIED

CI remoto
UNVERIFIED

Python metadata global 3.14.7
PLANNED / SEPARATE
```
