# Atlanticus — Roadmap

Estado: **CANONICAL BASELINE 1.0 — EXECUTION IN PROGRESS**

## Regla

Un solo foco por incremento.

Cerrar cada frontera con evidencia suficiente para su alcance.

No conservar legacy para sostener consumers o tests anteriores.

No mezclar cleanup transversal con el incremento funcional activo.

## Checkpoint publicado de referencia

```text
moragaga/atlanticus@6dd09a6f24370bbad8ae358b6d5d7c6ea9aeba4a
```

Parent:

```text
4e008055ddc551e6c08a7d87715340c8c7cd149e
```

## Hitos cerrados recientes

```text
PROFILES-CONFIGURATION-BOUNDARY-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Los hitos de Manager, Navigation generic configuration, Tools, KPI Configuration,
KPI Definition y ADA Configuration Manager cerrados anteriormente permanecen CURRENT.

## Hitos anteriores de Users superados

```text
USERS-STANDALONE-AUTHORITY-CUTOVER
CLOSED / SUPERSEDED BY USERS-GLOBAL-REGISTRY-ROOT-CUTOVER

USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER
SUPERSEDED / NOT FINAL TARGET

USERS-PROFILES-COMPOSITION-CUTOVER
SUPERSEDED AS PREVIOUS MODEL
```

Users ya no participa de Source/Projection Configuration.

## Frente Profiles

```text
PROFILES-CAPABILITY-EXTRACTION
IN PROGRESS
```

CURRENT parcial:

```text
profiles/core
CURRENT

profiles/configuration
CURRENT

ProfilesConfiguration ownership
profiles/configuration
```

No abrir este frente dentro del siguiente incremento de Users persisted data.

## Siguiente foco único

```text
USERS-PERSISTED-DATA-CUTOVER
PLANNED / NEXT
```

Primera etapa obligatoria:

```text
inspect actual persisted data/topology
classify current vs legacy records
identify Profiles information that must be preserved
freeze one-shot migration contract
```

Sólo después de evidencia suficiente puede implementarse una migración.

Target CURRENT que la migración deberá respetar:

```text
Blob registry
users/users.json.gz
atlanticus_users_registry / schema 1

Cosmos promoted Users
atlanticus_user / schema 1
```

No crear runtime adapters para `pending`, `resolved`, Users Source o combined
Users/Profiles configuration.

No borrar legacy persisted data antes de verificar que su información necesaria fue
migrada o preservada.

## Después de persisted data

```text
USERS-ADMINISTRATION-SURFACE-CUTOVER
PLANNED
```

Debe consumir `UsersAdministrationService` directamente y presentar lifecycle de
entidad, no Source/Projection.

Después, en incrementos separados:

```text
PROFILES-INDEPENDENT-SOURCE-LIFECYCLE
PLANNED

ACCESS-PROFILES-CONFIGURATION
PLANNED

NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
PLANNED
```

El orden exacto posterior puede refinarse sólo con evidencia CURRENT; no adelantar
implementación desde este documento.

## Open independiente

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN

PYTHON-METADATA-ALIGNMENT
PLANNED / OPEN

CI remote
UNVERIFIED

Concrete Entra/Graph UsersDirectoryReader provider
UNVERIFIED
```

## No mezclar en el siguiente chat

- Users Administration UI;
- Profiles Source/UI;
- Access;
- Navigation alignment;
- Python metadata cleanup;
- Web test cleanup global;
- unrelated Ruff cleanup;
- Command Center;
- Operational Data;
- rediseño de Manager core;
- rediseño de Source/Projection core.

Único foco:

```text
USERS-PERSISTED-DATA-CUTOVER
```
