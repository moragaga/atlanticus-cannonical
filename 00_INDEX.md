# Atlanticus Canonical Context — Index

Estado: **CURRENT — COMMAND CENTER CAPABILITY PARITY CLOSED; ADA ALARM ENGINE EXTRACTION DESIGN NEXT**

## Autoridad de este cierre

```text
Implementation        moragaga/atlanticus@346e7ac7ba7c21eede8b524613a6adee7e839e55
Canonical pre-replace moragaga/atlanticus-cannonical@19fe30dcc2f34dbe7a0c4615c188409ace2089b8
Git                   SOLO LECTURA
```

## Estado por frente

| Ubicación | Estado relevante |
|---|---|
| `01_CURRENT_STATE.md` | Command Center capability parity CLOSED; qualifier global permanece BLOCKED por una incompatibilidad Tools upstream separada. |
| `03_DECISIONS_CURRENT.md` | Users / Profiles / Navigation / Manager parity CURRENT; no adapters; ADA no se modifica dentro de este cierre. |
| `04_ALARM_ENGINE/` | Backend de Alarmas sigue físicamente bajo Command Center; extracción a `ada-alarm-engine` es el siguiente frente de diseño. |
| `14_ADA_COMMAND_CENTER/` | Paridad genérica implementada en `main`; backend Alarm queda como candidato completo a extracción. |
| `15_WEB_PLATFORM/` | Consumer parity de Command Center queda CURRENT/CLOSED para las capabilities tocadas. |
| `16_KPI_BACKEND_RECOVERY/` | KPI backend permanece CLOSED en su hito anterior; no se reabre aquí. |
| `17_DISTRIBUTION_AND_TOOLING/` | Frente separado; no se reabre aquí. |

## Checkpoints

```text
COMMAND-CENTER-USERS-PROFILES-NAVIGATION-MANAGER-PARITY   CLOSED / VERIFIED / CURRENT
COMMAND-CENTER-WEB-LOCK-NORMALIZATION                    CLOSED / VERIFIED / CURRENT
COMMAND-CENTER-FULL-WEB-QUALIFIER                        BLOCKED
ADA-ALARM-ENGINE-EXTRACTION-DESIGN                       PLANNED / NEXT
```

## Bloqueo separado del qualifier

El qualifier retomado después de la paridad alcanzó:

```text
generic-application   PASS
catalog-manager       PASS
catalog               FAIL
discovery-cosmos      FAIL
```

El fallo observado corresponde a coexistencia de tipos Tools de `ada.web.tools.*` y `ada.contracts.tools.*`.

No corregir ADA dentro del cierre de Command Center ni introducir adapters en Command Center para esconder esa incompatibilidad.

## Siguiente frontera única

```text
ADA-ALARM-ENGINE-EXTRACTION-DESIGN
```

Objetivo del próximo chat:

- inspeccionar todo `scopes/ada-command-center/backend`;
- comprobar la hipótesis de que ese backend constituye el Alarm Engine;
- clasificar dependencias como KEEP / MOVE / REMOVE / INVERT / REHOME;
- eliminar conceptualmente dependencias backend → Web;
- definir la frontera Command Center publication → Engine;
- congelar el dependency graph objetivo antes de mover código.

No implementar hasta cerrar debate/diseño.
