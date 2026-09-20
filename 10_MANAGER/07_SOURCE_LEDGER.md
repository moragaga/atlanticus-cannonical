# Manager — Source Ledger

Estado: **AUDIT LEDGER**

## Autoridad

- `moragaga/atlanticus:main` = realidad implementada.
- `moragaga/atlanticus-cannonical:main` = autoridad documental vigente.
- `moragaga/atlanticus-decisions` = HISTORICAL.
- Git permanece SOLO LECTURA para el asistente.

## Checkpoint CURRENT

```text
moragaga/atlanticus@31723a108ddd2f49346fdcbb844db9891eb08f4b
```

Parent:

```text
29bbf6d8f2b47a7d31e967ad4bb8de42f67a4c85
```

Tree:

```text
4213a0dd11abbc9cb22fb02ed4a60d08bf0f87c5
```

## Incremento cerrado

```text
ACCESS-UNRESTRICTED-PROFILES-CONTRACT
CLOSED / VERIFIED / CURRENT

ACCESS-MANAGER-UI-REVIEW
CLOSED / VERIFIED MANUAL / CURRENT
```

## Access contract CURRENT

```text
AdaAccessConfiguration
├── access_keys
└── profile_access
```

Semántica:

```text
root/local
→ todos los access_keys definidos
→ explicit grants forbidden

basic/guest/custom
→ explicit grants
```

No se agregó un schema nuevo para representar irrestricción.

## Access UI evidence

CURRENT incluye:

- tabs secundarios `Accesos / Perfiles`;
- creación visual `Ámbito + Permiso -> access_key`;
- paginación `10 / 20` para accesos y perfiles;
- fallback de catálogo de sistema para mostrar `basic` y `guest` cuando no hay Profiles
  Projection activa;
- `root` y `local` fuera de la asignación;
- filas compactas de perfiles con resumen y `Configurar`;
- modal viewport-centered;
- `dbc.Checkbox` para el editor de asignaciones;
- botón `Configurar` deshabilitado sin access keys;
- estado editable único en `AdaAccessConfiguration`;
- Source/Projection con el patrón visual ya usado en Navigation;
- containment local del dropdown de page size para evitar overflow horizontal;
- footer `Borrador local · accesos` alineado;
- ajuste mobile de filas de asignación.

El usuario confirmó manualmente el resultado visual final.

## ADA composition

ADA Configuration Manager compone Profiles con:

```text
title='Perfiles'
```

No se cambió el default generic de `compose_profiles_manager`.

## Qualification observada

Durante el hito:

```text
ADA Access Configuration
46 passed + 1 failing test
```

Ese único fallo correspondía a `dash.html.Input` y fue corregido a `dbc.Checkbox`.

El usuario confirmó después que todo quedó OK antes de publicar el checkpoint CURRENT.

También:

```text
ADA Configuration Manager
31 passed

git diff --check
PASS
```

Ruff package-wide del application package mostró tres `I001` en archivos no modificados por
este hito:

```text
kpi_definitions.py
kpis.py
workflows.py
```

No se corrigieron por estar fuera de alcance.

## Shared Manager CSS finding

El cambio previo en:

```text
web/capabilities/manager/src/atlanticus/web/manager/resources/css/10_surface.css
```

continúa CURRENT.

La correctness responsive/global transversal permanece para PHASE 2 del UI review.

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
IN PROGRESS / NEXT PAGE: PERFILES
```

`Herramienta` permanece OPEN / DEFERRED.

No abrir persistencia, Access runtime composition ni cleanup transversal durante esta frontera.
