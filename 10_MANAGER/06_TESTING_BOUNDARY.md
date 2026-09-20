# Manager — Testing Boundary

Estado: **CURRENT POLICY / UI REVIEW IN PROGRESS**

## Principio

Los tests automatizados deben proteger comportamiento, contracts, invariantes, regresiones y
flujos críticos.

No deben congelar presentación accidental ni implementación interna.

## Automatizar

Probar comportamiento real:

- autorización;
- registry/routing;
- lifecycle;
- draft persistence semantics;
- validation;
- source verification;
- conflict behavior;
- publish/project transitions;
- history semantics;
- callbacks críticos;
- errores;
- integridad referencial;
- session reuse;
- backend concurrency contract;
- paginación funcional: page calculation, límites, cambios de página y callbacks cuando sean
  contractualmente relevantes;
- invariantes de Access como grants prohibidos para `root/local`;
- helpers de identidad visual sólo cuando representan comportamiento explícito, no CSS;
- carga de un asset JS/CSS sólo cuando su presencia/carga sea parte explícita del contract.

## No congelar con tests

No añadir ni conservar tests cuya finalidad sea validar:

- CSS visual;
- margin/padding/color/tamaño;
- estilos concretos;
- clases CSS visuales concretas;
- estructura de markup sin comportamiento contractual;
- responsive visual;
- overflow visual;
- forma visual de paginación;
- contenido o estructura interna de JavaScript;
- existencia/no existencia de funciones internas;
- existencia/no existencia de clases internas;
- nombres privados;
- estructura accidental del package;
- una implementación interna concreta cuando el comportamiento observable ya está cubierto.

## Boundary tests válidos

Un boundary test puede validar imports/dependencies reales.

Ejemplo CURRENT:

```text
Navigation Configuration
must not import Profiles / Users / ADA
```

## UI review actual

```text
MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS
```

Orden:

```text
1. revisar presentación desktop/página
2. revisar responsive y media queries
3. ejecutar qualification final y limpiar tests inválidos
```

Navigation, Accesos y Perfiles ya tienen cierre visual manual.

El siguiente slice es Users.

No anticipar la fase 3 cambiando UI para complacer tests visuales.

## Qualification visual

Se valida visualmente:

- responsive;
- overflow;
- spacing;
- branding;
- header;
- modal shell;
- densidad;
- apariencia;
- alineación;
- consistencia entre superficies;
- presentación de paginación.

## Qualification Profiles observada

El usuario confirmó manualmente el resultado visual final publicado en:

```text
df5b99502265758e873e0565abf2176cc617104b
```

El commit contiene tests focales para:

```text
profile avatar initial
local identity avatar initials
pagination 10 / 20
source/projection labels
system profile context
composition metadata propagation
```

La existencia de esos tests en el commit no demuestra su ejecución.

Permanece:

```text
post-df5b targeted pytest
UNVERIFIED

post-df5b targeted Ruff
UNVERIFIED

remote CI
UNVERIFIED

full monorepo pytest
UNVERIFIED

full workspace Ruff
UNVERIFIED
```

## Después

```text
MANAGER-REAL-PERSISTENCE-QUALIFICATION
PLANNED / AFTER UI REVIEW
```

No usar Users UI review para abrir persistencia real ni otros frentes backend.
