# Manager — Canonical Index

Estado: **CURRENT GENERIC CORE / PROFILES COMPOSITION AVAILABLE**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_APPLICATION_BOUNDARY.md` | Manager como capability independiente. | CURRENT |
| `02_NAVIGATION_AND_HOME.md` | Home, sidebar y navegación administrativa. | CURRENT |
| `03_WORKFLOW_AND_SESSION.md` | WORKSPACE/SOURCE/PROJECTION y consumer final. | CURRENT |
| `04_TOOL_CONFIGURATION.md` | Tool Configuration y contrato Source/Projection. | FROZEN/CURRENT |
| `05_SOURCE_BLOB_HANDOFF.md` | Source/Projection consumido por Manager genérico. | CURRENT |
| `06_TESTING_BOUNDARY.md` | Testing contractual. | CURRENT POLICY |
| `07_SOURCE_LEDGER.md` | Fuentes/checkpoints/evidencia. | AUDIT LEDGER |
| `08_BOOTSTRAP_AND_ACCESS.md` | Bootstrap separado de Manager Access. | CURRENT |
| `09_ADA_COMPONENT_LINKS.md` | Links externos y warmup. | CONTRACT DESIGN |

## Contrato Manager CURRENT

```text
ManagerModule
├── source_key
├── source_service
├── source_reader_service
├── projection_service
├── draft_validation_service
├── source_history_service | None
└── access_key | None
```

Manager no declara legacy/exact dual contracts.

## Authorization CURRENT

```text
ManagerAuthorizationPolicy.can_view(principal, module)
```

No bypass por `is_local` ni profile administrator.

## Profiles Manager composition CURRENT

Existe una composition reusable:

```text
web/compositions/profiles-manager
```

Integra el lifecycle Profiles existente con Manager sin crear un contract paralelo.

Esto no implica que la aplicación final ADA Configuration Manager ya haya integrado todas
las superficies administrativas pendientes.

## Users

Users no es `ManagerModule` Source/Projection.

Users Administration continúa siendo un lifecycle administrativo separado.

## ADA Access

ADA Access ya tiene Source y Projection contract CURRENT.

Persistencia física de su Projection sigue pendiente antes de construir consumers que
dependan de durabilidad.

## Finding CURRENT

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

No añadir shim/alias.

## Siguiente frontera del Project

```text
ADA-ACCESS-PROJECTION-PERSISTENCE
PLANNED / NEXT / DESIGN FIRST
```

No es un cambio de Manager core.
