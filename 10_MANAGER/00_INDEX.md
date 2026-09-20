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
| `08_BOOTSTRAP_AND_ACCESS.md` | Bootstrap separado de Manager Access. | CURRENT |
| `09_ADA_COMPONENT_LINKS.md` | Links externos y warmup. | CONTRACT DESIGN |

## Autoridad de implementación verificada

```text
moragaga/atlanticus@29bbf6d8f2b47a7d31e967ad4bb8de42f67a4c85
```

Parent inmediato:

```text
856498c52f182cd531deae845c25bd51ae2ff4ea
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
├── Profiles
├── Accesos
├── Navegación
├── Herramienta
├── KPI
└── Definiciones KPI
```

## Navigation Manager Configuration CURRENT

Navigation generic mantiene Source/Projection reales y composition Manager reusable.

Navigation Configuration ya no depende de Profiles.

La composition acepta opcionalmente:

```text
profile_options_provider
```

y, cuando existe, instala validation basada en `NavigationProfileOption`.

ADA Configuration Manager hace la adaptación Profiles -> Navigation.

## UI review

```text
MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS
```

Slice cerrado:

```text
Navigation
CLOSED / VERIFIED MANUAL / CURRENT
```

Siguiente página:

```text
Herramienta
```

Orden dentro del review:

```text
1. visual/page consistency
2. responsive/media-query audit
3. test-contract cleanup + targeted qualification
```

## Qualification conocida

Antes de los últimos cambios visuales:

```text
Navigation core                           22 passed
Navigation Configuration                 42 passed
Navigation Manager                       10 passed
ADA Configuration Manager focused         5 passed
```

Post-`29bbf6d8...` targeted qualification:

```text
UNVERIFIED
```

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
