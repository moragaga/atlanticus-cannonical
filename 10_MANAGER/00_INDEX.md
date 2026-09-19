# Manager — Canonical Index

Estado: **CURRENT GENERIC CORE / AUTHORIZATION ALIGNED / UI RECOVERY NEXT**

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

Manager no declara:

```text
ManagerModuleAccess
workflow_service
exact_source_*
exact_projection_service
expected_source_revision
```

## Authorization CURRENT

```text
ManagerAuthorizationPolicy.can_view(principal, module)
```

Default:

```text
module.access_key in principal.access_keys
```

No bypass:

```text
is_local
administrator profile
```

## Configuration Manager CURRENT

Checkpoint:

```text
moragaga/atlanticus@9f12c41a23d69784c7c5b775a4093a94ac654d55
```

Surface CURRENT:

```text
navigation
tools
kpis                 optional
kpi-definitions      optional
```

Users no es `ManagerModule` Source/Projection.

Profiles y ADA Access tienen configuration lifecycle propio, pero no UI Manager CURRENT.

## Finding CURRENT

```text
web/compositions/navigation-manager
```

usa `authorization.can_access(...)`, incompatible con el protocolo CURRENT `can_view(...)`.

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

No añadir shim/alias.

## Siguiente frontera

```text
CONFIGURATION-UI-COMPOSITION-RECOVERY
PLANNED / NEXT
```

Manager shell/workflow sigue siendo generic. Cada editor concreto sigue siendo owned por su dominio.
