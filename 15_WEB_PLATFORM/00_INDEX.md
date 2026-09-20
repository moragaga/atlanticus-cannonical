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
moragaga/atlanticus@29bbf6d8f2b47a7d31e967ad4bb8de42f67a4c85
```

Canonical inspeccionado antes del reemplazo:

```text
moragaga/atlanticus-cannonical@deb493659b41c0d8fea5c70674002486b3b92cbc
```

Estado relevante:

```text
PROFILES-MANAGER-COMPOSITION
CLOSED / VERIFIED / CURRENT

USERS-PROFILES-CONTRACT-REALIGNMENT
CLOSED / VERIFIED / CURRENT

USERS-ADMINISTRATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROJECTION-PERSISTENCE
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

NAVIGATION-STANDALONE-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT

NAVIGATION-CONFIGURATION-UI-PASS
CLOSED / VERIFIED MANUAL / CURRENT

MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS

NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

Navigation Configuration no depende de Profiles core.

El siguiente trabajo continúa Manager UI page-by-page; no abre un nuevo backend contract.
