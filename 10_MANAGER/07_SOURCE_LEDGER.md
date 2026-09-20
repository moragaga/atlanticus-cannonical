# Manager — Source Ledger

Estado: **AUDIT LEDGER**

## Autoridad

- `moragaga/atlanticus:main` = realidad implementada.
- `moragaga/atlanticus-cannonical:main` = autoridad documental vigente.
- `moragaga/atlanticus-decisions` = HISTORICAL.
- Git permanece SOLO LECTURA para el asistente.

## Checkpoint CURRENT

```text
moragaga/atlanticus@df5b99502265758e873e0565abf2176cc617104b
```

Parent:

```text
31723a108ddd2f49346fdcbb844db9891eb08f4b
```

Tree:

```text
de1151ba72d44bc8ac6b6f2cfd6f57eb7e80c0a0
```

## Incremento cerrado

```text
PROFILES-MANAGER-UI-REVIEW
CLOSED / VERIFIED MANUAL / CURRENT
```

El cierre anterior de Access permanece:

```text
ACCESS-UNRESTRICTED-PROFILES-CONTRACT
CLOSED / VERIFIED / CURRENT

ACCESS-MANAGER-UI-REVIEW
CLOSED / VERIFIED MANUAL / CURRENT
```

## Profiles UI evidence

CURRENT incluye:

- Source y Projection visibles mediante metadata inyectada;
- defaults generic `Profiles Source` / `Profiles Projection`;
- provider local `Local Source` / `Local Projection`;
- provider Azure `Blob Storage` / `Cosmos DB`;
- ADA local `Local Source` / `In-process Projection`;
- paginación generic `10 / 20`, navegación numerada y summary;
- reserva visual de página controlada por CSS capability-local;
- empty state propio;
- modal capability-local respecto del viewport;
- preview del perfil y valores hex actuales en el editor;
- avatar de perfil normal con una inicial mayúscula;
- `local` como única excepción visual, mostrando identidades locales;
- identidad local con inicial del primer nombre + inicial del último nombre en mayúsculas;
- `Jane Doe` y `John Doe` representables ambos como `JD`, diferenciados por color;
- copy explicativo para system profiles;
- footer sin espacio vacío artificial.

El usuario confirmó manualmente el resultado visual final y declaró Perfiles cerrado.

## Profiles contract preservation

No se modificó el contrato durable de Profiles para soportar la presentación.

```text
ProfileDefinition
ProfileCatalog
ProfilesConfiguration
```

continúan siendo los contratos de dominio relevantes.

`compose_profiles_manager(...)` fue refinado para propagar metadata visible:

```text
description
source_name
projection_name
```

con defaults generic conservados.

## ADA local composition

CURRENT:

```text
title='Perfiles'
description='Define los perfiles disponibles y su presentación visual dentro del sistema.'
source_name='Local Source'
projection_name='In-process Projection'
```

## Qualification observada

VERIFIED:

```text
checkpoint CURRENT publicado
manual visual acceptance by user
```

UNVERIFIED:

```text
post-df5b targeted pytest
post-df5b targeted Ruff
remote CI
full monorepo pytest
full workspace Ruff
```

No declarar PASS automatizado sin salida observada.

## Shared Manager CSS finding

El cambio previo en:

```text
web/capabilities/manager/src/atlanticus/web/manager/resources/css/10_surface.css
```

continúa CURRENT.

La correctness responsive/global transversal permanece para una fase separada; no reabrirla
durante Users salvo defecto compartido demostrado.

## Conflict separado

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

`ManagerAuthorizationPolicy`:

```text
can_view(...)
```

Navigation Manager consumer:

```text
can_access(...)
```

No añadir alias.

## Próxima frontera

```text
USERS-MANAGER-UI-REVIEW
PLANNED / NEXT
```

`Herramienta` permanece `OPEN / DEFERRED`.

No abrir persistencia, Access runtime composition, Navigation authorization alignment ni
cleanup transversal durante Users.
