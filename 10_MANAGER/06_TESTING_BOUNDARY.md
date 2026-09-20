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

Navigation y Accesos ya tienen cierre visual manual.

El siguiente slice es Perfiles.

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

## Qualification Access observada

Antes de la última corrección mínima:

```text
ADA Access Configuration
46 passed + 1 failing test
```

El fallo fue `dash.html.Input`, corregido a `dbc.Checkbox`.

El usuario confirmó después que el resultado final quedó OK.

Además:

```text
ADA Configuration Manager
31 passed

git diff --check
PASS
```

Ruff package-wide de `ada-configuration-manager` reportó tres `I001` fuera del hito:

```text
kpi_definitions.py
kpis.py
workflows.py
```

No son evidencia contra Access y no deben mezclarse con el siguiente slice de Perfiles.

Sigue pendiente:

```text
remote CI
full monorepo pytest
full workspace Ruff
```

## Después

```text
MANAGER-REAL-PERSISTENCE-QUALIFICATION
PLANNED / AFTER UI REVIEW
```

No usar el UI review para abrir persistencia real ni otros frentes backend.
