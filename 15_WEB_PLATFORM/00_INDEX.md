# Atlanticus Web Platform — Canonical Index

Estado: **CURRENT / ADA GENERIC LOCAL BOOTSTRAP + MANAGER PERSISTENCE COMPOSITION CLOSED**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_CAPABILITY_INDEPENDENCE.md` | Fronteras de capabilities Web. | CURRENT / REFINED |
| `02_USER_ACTIVITY_HISTORY.md` | Historia ordenada por página y TTL 24 h. | CURRENT DIRECTION / CONTRACT DESIGN |
| `03_RESOURCE_PROVISIONING.md` | Provisionamiento Cosmos/Storage; plan Manager parcial implementado, inventario global abierto. | CURRENT / GLOBAL OPEN |
| `04_WEB_READINESS_AND_DECOUPLING.md` | Separación Web/capability, bootstrap ADA Generic y límites de qualification. | CURRENT / REFINED |
| `05_DEPLOYMENT_ORDER.md` | Web → preparación → Backend; implementación parcial local. | CURRENT DIRECTION / E2E OPEN |
| `06_PRE_MANAGER_BOOTSTRAP_SURFACE.md` | Superficie previa al Manager. | CURRENT DIRECTION |
| `07_PROJECTION_ORCHESTRATION.md` | Proyección determinística por dependencias. | CONTRACT DESIGN |
| `08_EXTERNAL_RESOURCE_REQUIREMENTS.md` | Recursos externos declarados por consumers. | CONTRACT DESIGN |
| `09_CURRENT_GAPS.md` | Estado implementado y qualification pendiente después de 1H.1. | CURRENT CHECKPOINT |
| `10_SOURCE_LEDGER.md` | Evidencia histórica más checkpoint de ADA Generic. | AUDIT LEDGER |
| `11_OPEN_ITEMS.md` | Siguiente foco único y pendientes explícitos. | NEXT QUALIFICATION |
| `12_USERS_PROFILES_NAVIGATION_CAPABILITY_BOUNDARY.md` | Fronteras CURRENT de Users/Profiles/Access/Navigation/Manager; UI y domain contracts. | CURRENT DECISION / REFINED |

## Autoridad y checkpoint de este corte

```text
Implementación CURRENT inspeccionada:
moragaga/atlanticus@ce1213ec14cdee0be905c042c1cf513d71fb5b2d

Canonical base inspeccionado antes del reemplazo:
moragaga/atlanticus-cannonical@6bd7f1f2616f954b422f3ddc1549a53a9b479682

Decisiones históricas consultadas:
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

Este checkpoint de plataforma **no sustituye la evidencia histórica** de otros cierres registrada en `10_SOURCE_LEDGER.md` o en `12_USERS_PROFILES_NAVIGATION_CAPABILITY_BOUNDARY.md`.

## Alcance cerrado en esta etapa

```text
ADA-STORAGE-NAMESPACE
CLOSED / VERIFIED / CURRENT

TOOL-PROJECTION-PERSISTENCE
CLOSED / VERIFIED / CURRENT

ADA-GENERIC-OPERATIONAL-BOOTSTRAP
CLOSED / VERIFIED LOCAL / CURRENT

ADA-WEB-KPI-COLLECTOR-OPERATIONAL-ATTACHMENT
CLOSED / VERIFIED LOCAL / CURRENT

ADA-GENERIC-INTEGRATED-MANAGER-LOCAL-BOOTSTRAP
CLOSED / VERIFIED LOCAL / CURRENT

ADA-GENERIC-MANAGER-DURABLE-ADAPTER-COMPOSITION
CLOSED / VERIFIED LOCAL / CURRENT

ADA-GENERIC-MANAGER-RESOURCE-CLI
CLOSED / VERIFIED LOCAL CONTRACT / CURRENT

ADA-GENERIC-COSMOS-CONTAINER-ENV-CUTOVER
CLOSED / VERIFIED LOCAL / CURRENT
```

La verificación reportada por el usuario para ADA Generic fue: **157 tests aprobados, Ruff, mirrors y wheel correctos**. No se ejecutó qualification contra Cosmos y Blob reales ni CI/monorepo global.

## Contratos existentes no alterados

`ManagerModule` representa una capability administrativa con Source/Projection; `ManagerEntry` integra administración sin Source/Projection ficticios. Users sigue como `ManagerEntry`, con promoción explícita individual, actualización inmediata y relación global `user -> profile_key`. Profiles es genérico; ADA Access es específico de aplicación; Navigation es genérico e independiente. `guest` puede ser transitorio, pero no un perfil administrativamente asignable; `local` es runtime-only.

## Siguiente foco único

```text
ADA-GENERIC-DOCKER-REAL-PERSISTENCE-QUALIFICATION
PLANNED / NEXT / UNVERIFIED
```

Probar el flujo existente con infraestructura real/emulada y registrar hallazgos; no abrir Command Center/alarmas, frontend adicional, Entra productivo ni cambios de backend en el mismo incremento.
