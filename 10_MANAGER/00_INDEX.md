# Manager — Canonical Index

Estado: **CURRENT / MANAGER CORE + ADA LOCAL CLOSED / USERS RECOVERY AND ADA EXTENSIONS PLANNED**

Corte estático de referencia del cierre: `moragaga/atlanticus@ebc7a8bf8d49e931fd4e2487dac5ee036011a0a5`. Evidencia Compose/labels reportada en commits anteriores; no atribuir ejecución monorepo al HEAD actual.

| Archivo | Contenido | Estado |
|---|---|---|
| `01_APPLICATION_BOUNDARY.md` | Manager capability genérica, sin acoplarse a ADA. | CURRENT |
| `02_NAVIGATION_AND_HOME.md` | Home, sidebar, header y navegación. | CURRENT |
| `03_WORKFLOW_AND_SESSION.md` | ManagerModule vs ManagerEntry, Source/Projection/workspace. | CURRENT; algunos checkpoints históricos |
| `04_TOOL_CONFIGURATION.md` | Tool Configuration y contratos Source/Projection. | CURRENT |
| `05_SOURCE_BLOB_HANDOFF.md` | Consumo de Source/Projection desde Manager. | CURRENT |
| `06_TESTING_BOUNDARY.md` | Testing de contratos y frontera visual. | CURRENT POLICY |
| `07_SOURCE_LEDGER.md` | Evidencia histórica de UI/integración local. | HISTORICAL; delta reciente en documentación de distribución |
| `08_BOOTSTRAP_AND_ACCESS.md` | Identidad, autorización, Users durable y nuevas fronteras de recovery/despliegue. | CURRENT + PLANNED |
| `09_ADA_COMPONENT_LINKS.md` | Links/warmup, contrato histórico del componente. | Revisar en su propio foco |
| `10_ADA_OPERATIONAL_IDENTIFICATION_BOUNDARY.md` | Nueva página ADA de cargo/área/grupo: diseño sin código. | PLANNED / NEW DOCUMENT |

## Invariantes CURRENT

Manager y Users genéricos conservan ownership propio. `ManagerEntry` incorpora Users sin fingir un Source/Projection de `ManagerModule`. `ManagerAuthorizationPolicy.can_view` autoriza visibilidad y las operaciones Source/Projection del Coordinator verifican privilegios. `/manager` es Home propia; cards/sidebar consumen un mismo registry autorizado. No confiar en cambios del navegador.

En ADA Manager vigente: Administración incluye Usuarios; Configuraciones incluye Perfiles, Accesos, Navegación, Herramienta, KPI y Definiciones KPI. Header ADA + Atlanticus sin nombre de usuario visible. Las etiquetas de proveedores se inyectan por composición: el patch reciente configura las variantes `local` y `durable`; tests reportados, visual nuevo artifact aún no revalidado.

## Cierres y límites

- Manager core/UI local: **CLOSED / VERIFIED PREVIO**.
- Composición durable y arranque real ADA Starter Compose `full`: **CURRENT / VERIFIED MANUAL + 33 TESTS ESPECÍFICOS REPORTADOS**; no equivale a productivo/Entra ni prueba completa de recuperación tras reinicio.
- Nuevo mecanismo especial de Users (validar/reconstruir desde registro aprobado): **PLANNED / NEXT**, diseño de frontera en `../15_WEB_PLATFORM/13_USERS_PROJECTION_RECOVERY.md`.
- Página de proyección aislada, fuera de Manager: **PLANNED**, ver `../15_WEB_PLATFORM/06_PRE_MANAGER_BOOTSTRAP_SURFACE.md`.
- Identificación operacional ADA: **PLANNED**, ver `10_ADA_OPERATIONAL_IDENTIFICATION_BOUNDARY.md`.

No abrir ni modificar Manager core durante el siguiente incremento; la prioridad es backend Users recovery.
