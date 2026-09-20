# Atlanticus Web Platform — Canonical Index

Estado: **CURRENT**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_CAPABILITY_INDEPENDENCE.md` | Fronteras CURRENT de Users / Profiles / ADA Access / Navigation. | CURRENT / REFINED |
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
| `12_USERS_PROFILES_NAVIGATION_CAPABILITY_BOUNDARY.md` | Boundary CURRENT Users/Profiles/ADA Access/Navigation/Manager. | CURRENT DECISION / REFINED |

Checkpoint de implementación CURRENT:

```text
moragaga/atlanticus@783d3578da52aeb5cf831999a7717dc8b79f2fb0
```

Canonical base reemplazada:

```text
moragaga/atlanticus-cannonical@0aa49ba1cdfc76754d4ec1ef4d169b6360f4da14
```

Estado relevante:

```text
PROFILES-MANAGER-COMPOSITION
CLOSED / VERIFIED / CURRENT

USERS-PROFILES-CONTRACT-REALIGNMENT
CLOSED / VERIFIED / CURRENT

USERS-ADMINISTRATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROFILE-OWNERSHIP-REALIGNMENT
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROJECTION-CONTRACT
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROJECTION-PERSISTENCE
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
PLANNED / NEXT / DESIGN FIRST

NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT

WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN

PYTHON-METADATA-ALIGNMENT
PLANNED / OPEN
```

ADA Access no tiene Web surface CURRENT.

El próximo diseño debe preservar que el consumo de access identifiers por funcionalidades
Web es manual/controlado por desarrolladores, no automático.
