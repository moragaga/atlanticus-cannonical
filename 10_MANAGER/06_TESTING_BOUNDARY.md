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

La implementación usa AST para inspeccionar imports.

SUPERSEDED:

```text
buscar substrings arbitrarios como "ada." en todo el source
```

porque puede coincidir con copy de UI sin representar una dependencia.

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

## Qualification automatizada conocida

Antes de la patch visual final de Navigation:

```text
Navigation core                           22 passed
Navigation Configuration                 42 passed
Navigation Manager                       10 passed
ADA Configuration Manager focused         5 passed
```

Post-checkpoint CURRENT:

```text
targeted pytest
UNVERIFIED

targeted Ruff
UNVERIFIED
```

No declarar final qualification hasta PHASE 3.

## Después

```text
MANAGER-REAL-PERSISTENCE-QUALIFICATION
PLANNED / AFTER UI REVIEW
```

No usar el UI review para abrir persistencia real ni otros frentes backend.
