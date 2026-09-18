# Atlanticus — Validation Baseline

Estado: **CURRENT**

## Regla

Qualification y tests son evidencia de propiedades del contrato CURRENT.

No son autoridad para conservar contratos, schemas o adapters SUPERSEDED.

No inventar un PASS cuando no existe resultado de ejecución observado.

## Autoridad de implementación

```text
moragaga/atlanticus@6dd09a6f24370bbad8ae358b6d5d7c6ea9aeba4a
```

Parent:

```text
4e008055ddc551e6c08a7d87715340c8c7cd149e
```

## Hitos contractuales relevantes

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-BOUNDARY-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CAPABILITY-EXTRACTION
IN PROGRESS
```

Los hitos genéricos de Manager, Navigation, Tools, KPI Configuration, KPI Definition
y ADA Configuration Manager previamente cerrados permanecen CURRENT.

## Users global registry root cutover — evidencia observada

Entorno reportado:

```text
Fedora WSL
Python 3.14.7
uv
```

Workspace Web:

```text
uv lock
PASS

uv sync
PASS

uv run pytest
416 passed / 7 skipped
```

Ruff scoped a superficies afectadas:

```text
capabilities/identity/core
capabilities/users/core
capabilities/users/blob
capabilities/users/cosmos
```

Resultado:

```text
PASS
```

El único finding de Ruff introducido por el cutover (`typing.Any` no usado en
Users Blob) fue eliminado en productivo y espejo comentado antes de publicar.

## ADA Configuration Manager — evidencia observada

Durante qualification apareció un test stale que todavía esperaba Users como módulo
0 y luego índices heredados del layout anterior.

Fue alineado al contrato CURRENT:

```text
navigation
tools
kpis
kpi-definitions
```

También se corrigieron dos findings E731 de `composition.py` reemplazando asignación
de lambda por `def actor_provider()` en productivo y espejo.

La qualification final scoped fue reportada por el usuario como OK antes del push.
El commit CURRENT remoto contiene esas correcciones.

No inventar un conteo final del package ADA que no fue capturado explícitamente en
la salida final del chat.

## Verificación remota del checkpoint

GitHub `main` apunta exactamente a:

```text
6dd09a6f24370bbad8ae358b6d5d7c6ea9aeba4a
```

El árbol remoto CURRENT confirma:

```text
users/activity
users/blob
users/core
users/cosmos
```

y ausencia de:

```text
users/configuration
users/projection-cosmos
compositions/users-manager
```

También confirma:

```text
UsersAccessResolver -> USER_NOT_PROMOTED without write
BlobUsersRegistryStore -> users/users.json.gz + ETag concurrency
CosmosUsersStore -> atlanticus_user schema 1
ADA Configuration Manager -> no Users module/services
```

## Política de tests Web

Probar:

```text
behavior
contracts
invariants
regressions
critical flows
```

No crear tests cuyo único objetivo sea:

```text
CSS visual
responsive
spacing
branding
apariencia
estructura visual
existencia/no existencia de funciones o clases
source token presence/absence
import presence/absence
AST/module structure
detalles internos
```

CSS/branding/responsive/spacing se validan visualmente salvo comportamiento
funcional automatizable.

Assets JS/CSS sólo se automatizan por existencia/carga cuando esa carga sea parte
real del contrato.

## Full Ruff workspace

Durante el proceso se observaron findings de Ruff fuera del scope del incremento en:

```text
capabilities/manager
capabilities/navigation/configuration/tests
ADA KPI/workflows files no modificados por este hito
```

No se aplicó `ruff --fix .` ni cleanup oportunista.

Estado final de full Ruff después del commit:

```text
UNVERIFIED / NOT CLAIMED PASS
```

## CI remoto

Para el commit CURRENT GitHub no reportó status checks ni workflow runs asociados.

Estado:

```text
UNVERIFIED
```

## Persisted data

No existe qualification en este cierre para:

```text
production Blob users registry
legacy Users/Profiles Source migration
legacy Cosmos pending/resolved cleanup
Blob <-> Cosmos parity in deployed environment
```

Estado:

```text
UNVERIFIED / NEXT FOCUS
```

## Entra discovery

`UsersDirectoryReader` existe como contrato.

No se verificó provider concreto de Microsoft Graph/Entra directory listing.

Estado:

```text
UNVERIFIED
```

## Python metadata

Canonical fija:

```text
Python 3.14.7
```

La qualification local se ejecutó bajo 3.14.7.

`web/pyproject.toml` remoto CURRENT todavía contiene:

```text
requires-python = "==3.14.2"
```

Estado:

```text
VERIFIED CONFLICT / OPEN
```

## Git

Git continúa SOLO LECTURA para el asistente.
