# Manager — Canonical Index

Estado: **CURRENT GENERIC CORE / PROFILES INTEGRATED / ADMIN COMPOSITION IN PROGRESS**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_APPLICATION_BOUNDARY.md` | Manager como capability independiente. | CURRENT |
| `02_NAVIGATION_AND_HOME.md` | Home, sidebar y navegación administrativa. | CURRENT |
| `03_WORKFLOW_AND_SESSION.md` | WORKSPACE/SOURCE/PROJECTION y consumers actuales. | CURRENT |
| `04_TOOL_CONFIGURATION.md` | Tool Configuration y contrato Source/Projection. | FROZEN/CURRENT |
| `05_SOURCE_BLOB_HANDOFF.md` | Source/Projection consumido por Manager genérico. | CURRENT |
| `06_TESTING_BOUNDARY.md` | Testing contractual. | CURRENT POLICY |
| `07_SOURCE_LEDGER.md` | Fuentes/checkpoints/evidencia. | AUDIT LEDGER |
| `08_BOOTSTRAP_AND_ACCESS.md` | Bootstrap separado de Manager Access. | CURRENT |
| `09_ADA_COMPONENT_LINKS.md` | Links externos y warmup. | CONTRACT DESIGN |

## Autoridad de implementación verificada

```text
moragaga/atlanticus@415c8263c15bae2b5d3c01b734b0f1e0101a7242
```

Parent:

```text
9b623d67e253413f6b0d894e10d9cc15735b553d
```

Tree:

```text
ff81bcd74b5e844604ca656562f9aaf398946d67
```

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

## Profiles Manager CURRENT

Existe la composition reusable:

```text
web/compositions/profiles-manager
```

Estado:

```text
PROFILES-MANAGER-COMPOSITION
CLOSED / VERIFIED / CURRENT

PROFILES-ADA-CONFIGURATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT
```

ADA Configuration Manager consume el `ManagerModule` producido por la composition existente.

La composition registra sus servicios mediante el `WebModule` ya asociado al `ManagerModule`,
sobre el `ServiceRegistry` real creado por Atlanticus Web.

No existe registry temporal, adapter, shim ni segundo contrato de integración.

Capability explícita:

```text
profiles.manage
```

## Users

Users no es `ManagerModule` Source/Projection.

CURRENT ya dispone de:

```text
UsersAdministrationService
```

pero no existe una composition/UI Manager equivalente publicada.

Por tanto:

```text
USERS-ADMINISTRATION-MANAGER-INTEGRATION
PLANNED / NEXT / DESIGN FIRST
```

La siguiente etapa debe partir del lifecycle administrativo Users existente.
No crear Source/Projection ficticios para hacerlo encajar en Manager.

## ADA Access

ADA Access Source/Projection es CURRENT.

Su Projection ya dispone de persistencia durable local y Cosmos, preservando el
`ProjectionTarget` y sus dependencies exactas.

Estado:

```text
ADA-ACCESS-PROJECTION-PERSISTENCE
CLOSED / VERIFIED / CURRENT
```

La UI/composition administrativa sigue pendiente:

```text
ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
PLANNED / AFTER USERS
```

## Finding CURRENT

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

`web/compositions/navigation-manager` conserva un consumer desalineado respecto de
`ManagerAuthorizationPolicy.can_view(...)`.

No añadir shim/alias.

## Secuencia congelada de continuación

```text
1. USERS-ADMINISTRATION-MANAGER-INTEGRATION
   PLANNED / NEXT

2. ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
   PLANNED / AFTER USERS

3. MANAGER-FINAL-ADMIN-COMPOSITION
   PLANNED / AFTER USERS + ADA ACCESS
```

No mezclar en esos incrementos:

```text
ADA Access runtime composition
Navigation operational authorization alignment
disabled-route surface
Python metadata alignment
global CI/test cleanup
```
