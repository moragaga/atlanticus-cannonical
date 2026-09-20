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
| `12_USERS_PROFILES_NAVIGATION_CAPABILITY_BOUNDARY.md` | Boundary CURRENT Users/Profiles/Access/Navigation/Manager. | CURRENT DECISION |

Checkpoint de implementación CURRENT:

```text
moragaga/atlanticus@df5b99502265758e873e0565abf2176cc617104b
```

Parent:

```text
31723a108ddd2f49346fdcbb844db9891eb08f4b
```

Canonical inspeccionado antes del reemplazo:

```text
moragaga/atlanticus-cannonical@07a0582c7acdd5c9b93f2a1bb02651e8c5302448
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
IN PROGRESS / NEXT PAGE: USERS

NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

Navigation Configuration no depende de Profiles core.

Profiles UI continúa owned por Profiles; su cierre no movió presentación al Manager.

El siguiente trabajo es Users UI page-by-page; no abre un nuevo backend contract ni convierte
Users en Source/Projection.
