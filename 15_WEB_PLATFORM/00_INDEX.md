# Atlanticus Web Platform — Canonical Index

Estado: **CURRENT**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_CAPABILITY_INDEPENDENCE.md` | Independencia técnica y fronteras CURRENT de Users global / Profiles / Navigation. | CURRENT / REFINED |
| `02_USER_ACTIVITY_HISTORY.md` | Historia ordenada por página y TTL 24 h. | CURRENT DIRECTION / CONTRACT DESIGN |
| `03_RESOURCE_PROVISIONING.md` | Provisionamiento de Cosmos/Storage y ownership. | CURRENT DIRECTION |
| `04_WEB_READINESS_AND_DECOUPLING.md` | Web disponible aun sin datos/backend/infra. | CURRENT DIRECTION |
| `05_DEPLOYMENT_ORDER.md` | Orden Web → preparación → Backend. | CURRENT DIRECTION |
| `06_PRE_MANAGER_BOOTSTRAP_SURFACE.md` | Superficie previa al Manager y autorización de bootstrap separada. | CURRENT DIRECTION |
| `07_PROJECTION_ORCHESTRATION.md` | Proyección determinística por dependencias. | CONTRACT DESIGN |
| `08_EXTERNAL_RESOURCE_REQUIREMENTS.md` | Cómo backend declara recursos sin depender de Web. | CONTRACT DESIGN |
| `09_CURRENT_GAPS.md` | Diferencias entre `main` y objetivos todavía abiertos. | CURRENT |
| `10_SOURCE_LEDGER.md` | Evidencia recuperada del código auditado. | AUDIT LEDGER |
| `11_OPEN_ITEMS.md` | Contratos todavía abiertos. | OPEN |
| `12_USERS_PROFILES_NAVIGATION_CAPABILITY_BOUNDARY.md` | Boundary CURRENT: Global Users independiente, Profiles generic, ADA Access app-specific y Navigation alineado a Profiles core. | CURRENT DECISION / REFINED |

Checkpoint de implementación de referencia:

```text
moragaga/atlanticus@3eb46dac80f23d438774e3afa39999dc96f592d7
```

Canonical base inspeccionado antes de este reemplazo:

```text
moragaga/atlanticus-cannonical@59ca0864daac7b79816974679cd4353033fe6408
```

Estado relevante:

```text
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
CLOSED / VERIFIED / CURRENT

Manager authorization stale administrator/local semantics
OPEN / PROPOSED NEXT

WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN

PYTHON-METADATA-ALIGNMENT
PLANNED / OPEN
```
