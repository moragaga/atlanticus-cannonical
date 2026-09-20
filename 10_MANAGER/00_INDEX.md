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
| `08_BOOTSTRAP_AND_ACCESS.md` | Bootstrap separado de Manager Access y ADA Access CURRENT. | CURRENT |
| `09_ADA_COMPONENT_LINKS.md` | Links externos y warmup. | CONTRACT DESIGN |

## Autoridad de implementación verificada

```text
moragaga/atlanticus@31723a108ddd2f49346fdcbb844db9891eb08f4b
```

Parent inmediato:

```text
29bbf6d8f2b47a7d31e967ad4bb8de42f67a4c85
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

## Navigation Manager Configuration CURRENT

Navigation generic mantiene Source/Projection reales y composition Manager reusable.

Navigation Configuration ya no depende de Profiles.

La composition acepta opcionalmente:

```text
profile_options_provider
```

y, cuando existe, instala validation basada en `NavigationProfileOption`.

ADA Configuration Manager hace la adaptación Profiles -> Navigation.

## ADA Access CURRENT

Access sigue siendo application-specific ADA.

CURRENT:

```text
access_keys
profile_access

root/local
unrestricted

basic/guest/custom
explicit grants
```

La UI Manager de Access está cerrada y usa los tabs `Accesos / Perfiles`, paginación generic
`10 / 20` y modal estable de asignación.

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
```

Siguiente página:

```text
Perfiles
```

`Herramienta` permanece OPEN / DEFERRED.

Orden dentro del review:

```text
1. visual/page consistency
2. responsive/media-query audit
3. test-contract cleanup + targeted qualification
```

## Qualification conocida

Access fue validado focalmente durante el hito y el usuario confirmó el resultado final antes
de publicar `31723a1...`.

También se observó:

```text
ADA Configuration Manager
31 passed
```

El Ruff package-wide de ese package mantiene tres `I001` fuera del hito Access.

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
