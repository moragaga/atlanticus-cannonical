# Distribution and Tooling — Index

Estado: **CURRENT / WEB STARTER PORTABLE HISTÓRICAMENTE CLOSED / NEW AUDIT NEXT**

Implementación inspeccionada para el traspaso: `moragaga/atlanticus@392ee281a32396516fb08c23c63514d8cbdb3489`. El cierre visual local de Manager/Navigation del 2026-09-27 no constituye requalification de artifacts, wheelhouses o Docker. Mantener separada la evidencia histórica del Starter publicada para `c2bf25e...`.

| Archivo | Contenido | Estado |
|---|---|---|
| `01_BACKEND_GENERATION.md` | Contract/Test → proceso → artifact. | CURRENT DIRECTION / NO REVALIDATION HERE |
| `02_FRONTEND_GENERATION.md` | Starter Generic/ADA editable, wheelhouse, qualification, brecha Starter Manager. | PORTABLE HISTÓRICO CLOSED / INTEGRATION OPEN |
| `03_ARTIFACT_DISTRIBUTION.md` | Artifact/distribution, Docker actual y fronteras siguientes. | CURRENT / DOCKER PARTIAL |
| `04_SCRIPTS_VALIDATION.md` | Gates por componente y master integrity. | CURRENT DIRECTION |
| `05_SUPPORT_SERVICES.md` | Cosmos/Storage como servicios de apoyo. | CURRENT DIRECTION / LOCAL COMPOSE INTEGRADO OPEN |
| `06_ENV_DETAIL.md` | Variables documentales, sin secretos; completitud por auditar. | CURRENT DIRECTION / TEMPLATES OPEN |
| `07_READMES.md` | README sólo al cierre del entregable. | CURRENT DIRECTION |
| `08_LOADERS.md` | Loaders por aplicación. | PRODUCT REQUIREMENT |
| `09_SOURCE_LEDGER.md` | Checkpoints y límites de evidencia. | AUDIT LEDGER |

## Cierre previo que no debe reabrirse

El header de Manager y los colores de Jane/John **en ADA Generic core local** fueron aceptados visualmente. Persisten en el código inspeccionado. Eso no cierra Manager, Navigation ni el recorrido HTML del **Starter ADA distribuido**, históricamente probado con Manager `disabled`.

## Próximo foco único en un nuevo chat

`DISTRIBUTION-ARTIFACTS-CURRENT-AUDIT` — **PLANNED / NEXT**: inspeccionar `atlanticus:main`, `atlanticus-cannonical:main`, reglas vigentes y scripts reales de generación; comprobar artifacts generables, closure de dependencias, lock y separación de ruedas propias/externas antes de acordar cualquier corrección. Backend/Web se revisarán como inventario del mismo contrato de distribución, sin implementar Docker ni `.env` en este primer incremento.

Después, en incrementos **separados**, revisar Docker multistage/host Gunicorn, archivos de entorno/pipeline y objetivo Compose local `infra/app/full`. Los nombres de estos modos son intención conversada, no una implementación acreditada. No sustituir el generador Compose **de procesos** ya existente ni inventar uno de Web sin auditar ambos y sus ownership.
