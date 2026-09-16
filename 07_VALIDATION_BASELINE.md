# Atlanticus — Validation Baseline

Estado: **CURRENT**

## Regla

Qualification y tests son evidencia de propiedades.

No reinterpretar un FAIL histórico como fallo vigente sin revisar su checkpoint y adjudicación.

No declarar GREEN global cuando sólo existe qualification scoped.

## Checkpoint actual de este cierre

```text
moragaga/atlanticus@d34cda3838a67907728b382e238f0178f9f1a64e
parent: 59fcd3ecc8f3441e64fbe0fc892b4467fa56f181
```

## Manager generic Source/Projection

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Sus contratos permanecen congelados.

## Navigation generic configuration cutover

```text
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Propiedades verificadas:

- Navigation construye un `ManagerModule` con la familia genérica de servicios;
- Source reader/publication/history comparten el workflow Navigation registrado;
- Source local compone `LocalSourceStore`;
- Source Azure compone `BlobSourceStore`;
- Projection local compone `LocalNavigationProjectionStore`;
- Projection Azure compone `CosmosNavigationProjectionStore`;
- no existe `expected_source_revision`;
- no existe `SOURCE_REVISION_STORE_ID`;
- no existe reconstrucción revision→`ProjectionTarget`;
- adapters/configuration stores legacy fueron eliminados;
- no se introdujeron shims/aliases de compatibilidad.

## Qualification observada

Ejecutada por el usuario en el workspace real:

```text
Python 3.14.2

ruff scoped
All checks passed!

pytest scoped
102 passed in 0.32s

forbidden legacy scan
0 results

git diff --check
PASS

git diff --cached --check
PASS
```

`uv lock` resolvió 78 paquetes y reconoció los nuevos paquetes de Navigation.

## Full Web suite

Comando:

```text
cd web
uv run pytest
```

Resultado:

```text
BLOCKED DURING COLLECTION
4 collection errors
```

Los cuatro errores visibles se originan al importar `web/compositions/users-manager`, inicialmente por:

```text
ExactSourceHistoryReadResult
```

que no existe en el Manager CURRENT.

No hubo evidencia de fallo funcional de Navigation en esa ejecución porque la suite global no llegó a ejecutarse.

## Adjudicación

VERIFIED:

- `users-manager` no fue modificado por el commit de Navigation;
- la desalineación existía en el parent del commit actual;
- `users-manager` conserva imports/contratos `Exact*`;
- Manager CURRENT expone `SourceReadResult`, `SourceHistoryReadResult`, `SourcePublicationResult` y el Projection contract genérico.

UNVERIFIED:

- la solución exacta para Users;
- si los archivos `exact_*` deben ser eliminados, renombrados o reemplazados por una composición distinta;
- qualification global después de resolver Users.

## Regla para el siguiente chat

No implementar por inferencia desde el error.

Primero:

```text
USERS-MANAGER-ALIGNMENT-VALIDATION
```

Debe producir:

```text
implementation inspected in atlanticus:main
canonical intent inspected in atlanticus-cannonical:main
exact mismatch enumerated
current contract identified
legacy/current ownership adjudicated
implementation decision only after evidence
```

## UNVERIFIED global

- full Web GREEN;
- full ADA suite;
- Docker E2E;
- CI remoto adicional;
- Python 3.14.7/Trixie global;
- consumers Tools/KPI Configuration/KPI Definition.

## Git

Git continúa READ ONLY para el asistente salvo autorización explícita.
