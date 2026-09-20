# Manager — Canonical Index

Estado: **CURRENT GENERIC CORE / FINAL ADMIN COMPOSITION INTEGRATED / UI REVIEW IN PROGRESS**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_APPLICATION_BOUNDARY.md` | Manager como capability independiente. | CURRENT |
| `02_NAVIGATION_AND_HOME.md` | Home, sidebar, navegación administrativa y Navigation configuration UI boundary. | CURRENT |
| `03_WORKFLOW_AND_SESSION.md` | WORKSPACE/SOURCE/PROJECTION para módulos y frontera de entries administrativos. | CURRENT |
| `04_TOOL_CONFIGURATION.md` | Tool Configuration y contrato Source/Projection. | FROZEN/CURRENT |
| `05_SOURCE_BLOB_HANDOFF.md` | Source/Projection consumido por Manager genérico. | CURRENT |
| `06_TESTING_BOUNDARY.md` | Testing contractual y frontera visual. | CURRENT POLICY |
| `07_SOURCE_LEDGER.md` | Fuentes/checkpoints/evidencia. | AUDIT LEDGER |
| `08_BOOTSTRAP_AND_ACCESS.md` | Bootstrap, Manager Access y slices UI cerrados. | CURRENT |
| `09_ADA_COMPONENT_LINKS.md` | Links externos y warmup. | CONTRACT DESIGN |

## Autoridad de implementación verificada

```text
moragaga/atlanticus@df5b99502265758e873e0565abf2176cc617104b
```

Parent inmediato:

```text
31723a108ddd2f49346fdcbb844db9891eb08f4b
```

Tree:

```text
de1151ba72d44bc8ac6b6f2cfd6f57eb7e80c0a0
```

## Contratos Manager CURRENT

### ManagerModule

`ManagerModule` representa una capability administrativa respaldada por Source/Projection.

### ManagerEntry

`ManagerEntry` representa una capability administrativa visible en el mismo shell Manager sin
exigir lifecycle Source/Projection ficticio.

`ManagerModule` y `ManagerEntry` comparten navegación, routing, authorization y WebModule
lifecycle; sólo `ManagerModule` participa del coordinator Source/Projection.

## Authorization CURRENT

```text
ManagerAuthorizationPolicy.can_view(principal, item)
```

No existe bypass implícito por:

```text
principal.is_local
profile administrator
```

## ADA Configuration Manager CURRENT

```text
Administración
└── Users

Configuraciones
├── Perfiles
├── Accesos
├── Navegación
├── Herramienta
├── KPI
└── Definiciones KPI
```

`Perfiles` es el título application-specific usado por ADA para la capability generic Profiles.
El default generic no fue renombrado.

## Profiles CURRENT

Profiles sigue siendo generic Atlanticus y mantiene ownership de su UI.

CURRENT:

```text
source/projection context
composition-driven

pagination
10 / 20

profile editor
viewport-centered capability-local modal

configured preview
single uppercase initial

local presentation
local identities with first+last initials
```

La composition reusable acepta `description`, `source_name` y `projection_name` sin cambiar
los defaults generic.

ADA local usa:

```text
title = Perfiles
source = Local Source
projection = In-process Projection
```

El usuario confirmó manualmente el resultado final.

## Navigation Manager Configuration CURRENT

Navigation generic mantiene Source/Projection reales y composition Manager reusable.

Navigation Configuration no depende de Profiles.

La composition acepta opcionalmente `profile_options_provider` y ADA adapta Profiles al
contrato neutral de Navigation.

## ADA Access CURRENT

Access sigue siendo application-specific ADA.

```text
root/local
unrestricted

basic/guest/custom
explicit grants
```

La UI Manager de Access permanece cerrada y aceptada manualmente.

## UI review

```text
MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS
```

Slices cerrados:

```text
Navigation
CLOSED / VERIFIED MANUAL / CURRENT

Accesos
CLOSED / VERIFIED MANUAL / CURRENT

Perfiles
CLOSED / VERIFIED MANUAL / CURRENT
```

Siguiente página:

```text
Users
```

`Users` es `ManagerEntry`; no añadirle Source/Projection para uniformar la UI.

`Herramienta` permanece `OPEN / DEFERRED`.

El usuario indicó que después de Users se cerrará el trabajo actual de Manager por ahora;
frentes diferidos conservan su estado.

## Qualification conocida

Para el checkpoint `df5b995...`:

```text
Profiles visual review
VERIFIED MANUAL

post-df5b targeted pytest
UNVERIFIED

post-df5b targeted Ruff
UNVERIFIED
```

La qualification transversal completa continúa pendiente.

## Finding separado

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

No añadir shim/alias.

## Después del UI review

```text
MANAGER-REAL-PERSISTENCE-QUALIFICATION
PLANNED
```
