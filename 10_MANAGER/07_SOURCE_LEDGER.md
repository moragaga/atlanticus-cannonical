# Manager — Source Ledger

Estado: **AUDIT LEDGER**

## Autoridad

- `moragaga/atlanticus:main` = realidad implementada.
- `moragaga/atlanticus-cannonical:main` = autoridad documental vigente.
- `moragaga/atlanticus-decisions` = HISTORICAL.
- Git permanece SOLO LECTURA para el asistente.

## Checkpoint CURRENT

```text
moragaga/atlanticus@29bbf6d8f2b47a7d31e967ad4bb8de42f67a4c85
```

Parent:

```text
856498c52f182cd531deae845c25bd51ae2ff4ea
```

Tree:

```text
3f27ad599c6dec610dff5317494a73b276d2ebc4
```

## Incremento cerrado

```text
NAVIGATION-STANDALONE-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Cambios de ownership/boundary:

```text
Navigation Configuration -> Profiles core
REMOVED

NavigationProfileOption
CURRENT

NavigationProfileOptionsProvider
CURRENT / OPTIONAL

Profiles adaptation
APPLICATION/COMPOSITION BOUNDARY
```

## Navigation access semantics

```text
allowed_profiles = ()
PUBLIC WITHIN NAVIGATION AUTHORIZATION

allowed_profiles = non-empty
RESTRICTED

enabled = False
DENY
```

## ADA composition

ADA Configuration Manager adapta `ProfileCatalog` a `NavigationProfileOption`.

No expone `root` ni `local` como opciones asignables de Navigation.

## Navigation UI evidence

CURRENT incluye:

- top-level pagination `10 / 20`;
- sections collapsed initially;
- all children shown on expansion;
- empty state centered in reserved page area;
- page-size dropdown focus-target containment;
- no standalone profiles card;
- no guest auto-selection.

El usuario confirmó manualmente que el resultado visual final quedó correcto.

## Qualification observada

Antes de los ajustes visuales finales:

```text
Navigation core                           22 passed
Navigation Configuration                 42 passed
Navigation Manager                       10 passed
ADA Configuration Manager focused         5 passed
```

El checkpoint final también contiene el reemplazo del boundary test textual por inspección AST.

No se observó una ejecución post-`29bbf6d8...` completa en este cierre.

## Shared Manager CSS finding

`29bbf6d8...` también modifica:

```text
web/capabilities/manager/src/atlanticus/web/manager/resources/css/10_surface.css
```

Cambio CURRENT:

```text
module-page bottom padding -> 0
mobile module-page bottom padding -> 0
```

La correctness responsive/global permanece UNVERIFIED y se revisará en la fase 2 del UI
review.

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
MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS / NEXT PAGE: HERRAMIENTA
```

No abrir persistencia ni runtime authorization durante esta frontera.
