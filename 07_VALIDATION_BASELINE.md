# Atlanticus — Validation Baseline

Estado: **CURRENT**

## Regla

Qualification y tests son evidencia de propiedades.

No reinterpretar un FAIL histórico como fallo vigente sin revisar su adjudicación.

No declarar GREEN global cuando sólo existe qualification scoped.

Un hito puede quedar CLOSED/VERIFIED dentro de su frontera aunque una suite mayor tenga fallos adjudicados a contratos preexistentes y fuera de alcance; esos fallos deben quedar explícitamente BLOCKED/OPEN, no ocultos.

## Checkpoint actual

```text
moragaga/atlanticus@384a68fe8fa42263623c95d1d132af2ca54574c8
parent: b2254450b4543d2422ca8580357b9054b515cd6e
```

`atlanticus:main` fue verificado read-only apuntando exactamente a ese commit.

## Users exact Manager lifecycle

Estado:

```text
USERS-EXACT-MANAGER-LIFECYCLE
CLOSED / VERIFIED / CURRENT
```

Propiedades verificadas por implementación + qualification scoped:

- módulo Users no declara `workflow_service`;
- validation separada y canónica;
- exact Source read;
- exact Source publication;
- exact Projection status;
- exact Projection target/execution;
- exact Source History list/read;
- History preview canónico;
- History release cargada como trabajo local sobre BASE current;
- `UsersManagerWorkflowAdapter` removido;
- no `SourceReleaseId -> str` shim;
- no `HistoryPage -> RevisionHistoryEntry` adapter;
- product/commented mirrors equivalentes en el alcance modificado.

## Qualification observada del cierre

Reportada en workspace real:

```text
Manager + Users Configuration + users-manager focused suite   238 passed
ADA full suite                                                56 passed
ADA full suite                                                4 failed
```

Los 4 failures ADA fueron adjudicados a adapters legacy no-Users:

```text
KpiConfigurationManagerWorkflowAdapter
KpiDefinitionManagerWorkflowAdapter
ToolConfigurationManagerWorkflowAdapter
NavigationManagerWorkflowAdapter
```

Síntomas verificados:

- `_projection(...)` intenta construir `ProjectionExecutionResult(source_revision=...)`;
- Manager vigente exige `ProjectionExecutionResult.target`;
- adapters legacy exponen `project(expected_source_revision: str)` en vez de `project(ProjectionTarget)`;
- `ConfigurationLifecycleWorkflow` vigente exige `get_current_projection_target()`.

Por tanto:

```text
Users exact lifecycle qualification     GREEN / VERIFIED
ADA full suite                          NOT GREEN
ADA global blocker                      LEGACY PROJECTION CONTRACT ALIGNMENT
```

No corregir esos adapters dentro del hito Users ya cerrado.

## Historial de qualification preservado

Continúan válidos como evidencia histórica, sin sustituir current checkpoint:

- Source Core/Local/Blob;
- Projection Core;
- Users Cosmos Projection;
- Manager root exact-target;
- Profiles extraction/baseline;
- UCS-1;
- Admin Composition Backend;
- Manager Exact-Source Boundary;
- Users Admin Draft Baseline;
- Users Manager Exact-Source Composition;
- Admin UI Draft Cutover.

Sus conteos históricos deben consultarse en commits/documentación de cada hito; no mezclarlos con la qualification de `384a68fe...`.

## Lo que current checkpoint NO demuestra

UNVERIFIED:

- full Web suite completa en `384a68fe...`;
- full ADA suite GREEN;
- Docker E2E;
- constructor/composition root externo real que inyecta `users_profiles_administration`;
- constructor/composition root externo real que inyecta `users_exact_projection`;
- qualification visual browser productiva del History exacto, salvo evidencia separada;
- Python 3.14.7 qualification del checkpoint;
- CI remoto adicional.

## Python baseline

Decisión Project:

```text
Python 3.14.7
python:3.14.7-slim-trixie
```

El repositorio aún contiene paquetes Web con `requires-python ==3.14.2`.

No presentar el cierre actual como qualification del baseline final 3.14.7.

## Git / CI

Git continúa READ ONLY para el asistente salvo autorización explícita.

No se afirma CI remoto adicional para `384a68fe...`.

La evidencia funcional citada proviene del workspace real reportado por el usuario y de inspección read-only de `atlanticus:main`.
