# Atlanticus Web Platform — Canonical Index

Estado: **CURRENT / CORE HISTORICAL CHECKPOINTS + WEB STARTER PORTABLE CLOSED / MANAGER STARTER OPEN**

Este delta documenta exclusivamente el frente Web Starter inspeccionado en `moragaga/atlanticus@c2bf25e353b890dc8fd8553ad375745d23ec7154`; no sustituye los checkpoints de otras capacidades ni declara una requalification global.

| Archivo | Contenido | Estado |
|---|---|---|
| `01_CAPABILITY_INDEPENDENCE.md` | Ownership de capacidades Web independientes. | CURRENT |
| `02_USER_ACTIVITY_HISTORY.md` | Historial de usuario por página/TTL. | CURRENT DIRECTION |
| `03_RESOURCE_PROVISIONING.md` | Cosmos/Storage, plan Manager parcial, plan global pendiente. | CURRENT / GLOBAL OPEN |
| `04_WEB_READINESS_AND_DECOUPLING.md` | Arranque resiliente y disponibilidad separada de capabilities. | CURRENT |
| `05_DEPLOYMENT_ORDER.md` | Web → preparación/proyección → Backend. | CURRENT DIRECTION / E2E OPEN |
| `06_PRE_MANAGER_BOOTSTRAP_SURFACE.md` | Login real y consola de bootstrap previa a Manager. | CURRENT DIRECTION |
| `07_PROJECTION_ORCHESTRATION.md` | Plan determinístico por dependencias. | CONTRACT DESIGN |
| `08_EXTERNAL_RESOURCE_REQUIREMENTS.md` | Declaración de recursos externos. | CONTRACT DESIGN |
| `09_CURRENT_GAPS.md` | Gaps después de SOURCE_SMOKE/PORTABLE y Docker local parcial. | CURRENT CHECKPOINT |
| `10_SOURCE_LEDGER.md` | Evidencia histórica previa; nuevo delta en `17_DISTRIBUTION_AND_TOOLING/09_SOURCE_LEDGER.md`. | HISTORICAL |
| `11_OPEN_ITEMS.md` | Próximo foco Web Starter Manager/Navigation/visual y otros pendientes. | CURRENT / NEXT |
| `12_USERS_PROFILES_NAVIGATION_CAPABILITY_BOUNDARY.md` | Contratos de Users/Profiles/Access/Navigation/Manager. | CURRENT / REFINED |

Estados relevantes de este delta:

```text
Generic / ADA Starter SOURCE_SMOKE      CLOSED / VERIFIED MANUAL
Generic / ADA Starter PORTABLE          CLOSED / VERIFIED MANUAL
Docker images build + health/live       VERIFIED MANUAL / PARTIAL
Starter Manager/header/sidebar          OPEN
ADA /example browser HTML access        OPEN / FINDING
Docker production Gunicorn/8000         PROPOSED / PLANNED
Cosmos/Azurite and Entra E2E            PLANNED / UNVERIFIED
```

El próximo foco coherente es el recorrido de Manager + Navigation y su qualification visual en el Starter, sin reabrir el core por defecto ni abrir todavía Docker productivo, secretos o emuladores.
