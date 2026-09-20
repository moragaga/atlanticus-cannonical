# Atlanticus Web Platform — Canonical Index

Estado: **CURRENT**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_CAPABILITY_INDEPENDENCE.md` | Fronteras de capabilities Web. | CURRENT / REFINED |
| `02_USER_ACTIVITY_HISTORY.md` | Historia ordenada por página y TTL 24 h. | CURRENT DIRECTION / CONTRACT DESIGN |
| `03_RESOURCE_PROVISIONING.md` | Provisionamiento de Cosmos/Storage y ownership. | CURRENT DIRECTION |
| `04_WEB_READINESS_AND_DECOUPLING.md` | Web disponible aun sin datos/backend/infra. | CURRENT DIRECTION |
| `05_DEPLOYMENT_ORDER.md` | Orden Web → preparación → Backend. | CURRENT DIRECTION |
| `06_PRE_MANAGER_BOOTSTRAP_SURFACE.md` | Superficie previa al Manager. | CURRENT DIRECTION |
| `07_PROJECTION_ORCHESTRATION.md` | Proyección determinística por dependencias. | CONTRACT DESIGN |
| `08_EXTERNAL_RESOURCE_REQUIREMENTS.md` | Recursos externos declarados por consumers. | CONTRACT DESIGN |
| `09_CURRENT_GAPS.md` | Diferencias entre `main` y objetivos abiertos. | CURRENT |
| `10_SOURCE_LEDGER.md` | Evidencia recuperada del código auditado. | AUDIT LEDGER |
| `11_OPEN_ITEMS.md` | Contracts todavía abiertos. | OPEN |
| `12_USERS_PROFILES_NAVIGATION_CAPABILITY_BOUNDARY.md` | Boundary CURRENT Users/Profiles/Access/Navigation/Manager. | CURRENT DECISION / REFINED |

Checkpoint de implementación CURRENT:

```text
moragaga/atlanticus@ce07ada07e3f4f100b97ad2ac5e7285b54419c20
```

Parent:

```text
df5b99502265758e873e0565abf2176cc617104b
```

Tree:

```text
825dbaffba30b42199c54dbd4da9ba234f3ef437
```

Canonical inspeccionado antes de este reemplazo:

```text
moragaga/atlanticus-cannonical@50e364bc4bd61b1ecbd9c3aebb5ad4b8e4ee8e4f
```

Estado relevante:

```text
PROFILES-MANAGER-COMPOSITION
CLOSED / VERIFIED / CURRENT

PROFILES-MANAGER-UI-REVIEW
CLOSED / VERIFIED MANUAL / CURRENT

USERS-PROFILES-CONTRACT-REALIGNMENT
CLOSED / VERIFIED / CURRENT

USERS-ADMINISTRATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

USERS-MANAGER-UI-REVIEW
CLOSED / CURRENT / ACCEPTED WITH NON-BLOCKING POLISH

USERS-GUEST-ASSIGNMENT-BOUNDARY
CURRENT / IMPLEMENTED

ADA-ACCESS-PROJECTION-PERSISTENCE
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

ACCESS-MANAGER-UI-REVIEW
CLOSED / VERIFIED MANUAL / CURRENT

NAVIGATION-STANDALONE-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT

NAVIGATION-CONFIGURATION-UI-PASS
CLOSED / VERIFIED MANUAL / CURRENT

MANAGER-UI-CONSISTENCY-REVIEW
CLOSED FOR CURRENT V1 / NON-BLOCKING POLISH DEFERRED

NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

Users continúa siendo una `ManagerEntry`: no recibe Source/Projection sintético ni workflow global de
borrador/publicación.

La administración de Users opera con commits explícitos por usuario:

```text
Promover
→ operación inmediata de promoción

Editar → Guardar
→ operación inmediata de actualización
```

`guest` puede representar estado transitorio previo a la promoción, pero no es un perfil
administrativo asignable. `local` continúa siendo runtime-only.

La qualification automática posterior al correctivo final de Users y la qualification de wiring
productivo Blob/Cosmos permanecen explícitamente abiertas; no bloquean el cambio de foco hacia
backend ADA, pero tampoco deben declararse verificadas sin evidencia.
