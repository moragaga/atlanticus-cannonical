# Alarm Engine — Open Items

Estado: **B2a/B2b CLOSED LOCAL E INTEGRADOS EN MAIN / B2c SIGUIENTE FOCO PLANNED / INFRAESTRUCTURA REAL UNVERIFIED**.

Corte: `atlanticus@ebc7a8bf8d49e931fd4e2487dac5ee036011a0a5`, canonical consultado `be2c424c44648e6488daae36d410cf425eed02b8`, decisions `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`.

## CLOSED / CURRENT confirmados para esta transición

- A y B1: Source v3 Rn/Cn exacto, resolver B.2, publicación READY/BLOCKED local, lector compartido READY/exacto, `AlarmConfigurationArtifactRef` y planificador sobre unión de identidades definidas.
- B2a.1: V1 adopción global durable de cero grupos en WAL, sin snapshot ficticio; cadenas y crash/recovery probados localmente.
- B2a.2: V2 adopción global con 1..N grupos, references exactas, confirmación y replay agrupados. V1 sigue siendo formato actual, no legacy.
- B2b.1: `AlarmEffectiveConfigurationHead` `alarm-effective-head.v1`, proyección en `runtime/state/effective-head.json`, validación/reparación tras recovery y bloqueo de lectura incoherente.
- B2b.2: Runtime obtiene `RuntimeEffectiveConfiguration(effective_head,revision)` desde `AlarmPersistence.read_effective_head()` y el lector exacto; comprueba pin completo y puede detectar sustitución posterior. READY sigue disponible sólo como candidata de nuevas adopciones.
- Gates locales de B2a/B2b: tests, Ruff, formato y builds según evidencia delimitada en `08_QUALIFICATION_BASELINE.md`; código contrastado en HEAD de Git. No adjudicar CI/E2E.

## OPEN verificables: qué falta y por qué

| Elemento | Estado | Motivo / condición de salida | Frente |
|---|---|---|---|
| Ejecutar `ADDED`/`ENABLED`/`REMOVED` desde source disabled | **PLANNED / OPEN** | B1 los planifica, executor vigente exige source plan ejecutable; diseñar reconciliación explícita y tests de estado/grupos. | **B2c.1 — próximo foco único**. |
| Conectar el executor a adopción V1/V2 y EFFECTIVE | **PLANNED / OPEN** | `adoption_execution.py` aún usa `composition.commit_batch` y sin grupos devuelve sin commit; debe confirmar globalmente también Delivery-only. | B2c.2, tras acordar B2c.1. |
| Bootstrap inicial del ejecutor | **PLANNED / OPEN** | Persistence soporta primera V1/V2, pero no hay flujo integral demostrado que materialice/active primera versión con executor/job. No usar latest READY como autoridad tras arranque. | B2c, según diseño. |
| Enlace del job operacional a selección EFFECTIVE | **PLANNED / OPEN** | B2b.2 aporta lector/revalidación explícitos; job/composition actual no los invoca como flujo integrado demostrado. | B2c posterior a contratos. |
| Evaluator/key, kind, priority_group change | **OPEN / CONFLICT** | Decisions B.1 contempla COMPATIBLE o migración de dos grupos; planner MAIN rechaza. Precisa decisión de alcance propia. | No ampliar B2c.1 automáticamente. |
| Migración de `priority_group` para Rules deshabilitadas con estado residual | **OPEN / ACCEPTED RISK MVP** | B1 conserva identidades definidas pero no el grupo de una Rule deshabilitada. Si se cambia su grupo al reactivarla, el planner puede no detectarlo y una desactivación residual podría permanecer en el grupo anterior. El MVP prohíbe operativamente ese cambio, no incorpora búsqueda global ni migración y acepta expresamente el riesgo hasta una evolución dedicada. | Migración de `priority_group` posterior a B2c. |
| Reappearance sobre `ManagementEffect` vivo | **OPEN** | Cambio de timer/special_conditions del plan necesita reconciliación de estado vivo verificable. | Incremento dedicado de Adoption después del básico. |
| `resolution_key_at_start` en occurrence | **PLANNED** | Estado actual conserva revisiones Alarm/Tool separadas; identidad exacta al inicio aún no implementada. | Contrato dedicado si se confirma. |
| Producción real de evaluator qualifications y Tool GREEN | **UNVERIFIED / OPEN** | `AlarmEvaluatorRegistry` y JSON controlled qualification no son pipeline operacional real. | Integración futura, fuera de B2c.1. |
| Loader de fuentes/ejecución E2E | **PLANNED** | Core/session/iteration tienen puertos `atlanticus.operational_data`; conexión a fuentes reales y evaluadores reales del job no está probada aquí. | Después de B2c. |
| History / evidencia operacional / Delivery / Management Capture | **PLANNED / SEPARATE** | Existen hechos/evidencias/acciones en Core y WAL, pero falta su flujo real y contratos externos exactos. No duplicar Engine. | Focos posteriores. |
| Materialization/Runtime sobre Cosmos/Blob y volumen final multi-host | **UNVERIFIED / BLOCKED POR ENTORNO** | Los gates unitarios/locales no prueban producción, sharing, rename/fsync interhost ni takeover físico. | Qualification operacional cuando exista entorno. |
| Cualquier lectura externa directa de snapshots durante V2 | **OPEN para futuro consumidor** | Los snapshots físicos se escriben individualmente; el cursor materialized es agrupado, pero raw reads no son MVCC. | Contrato de lector Live/History cuando corresponda. |
| Migración desde histórico durable exclusivamente de grupos previo a primera adopción | **OPEN / CONDICIONAL** | Persistence rechaza primera adopción sobre histórico no migrado; no hay migrador ni inventario de datasets reales que lo requieran. | Sólo si rollout demuestra necesidad. |
| Routing visual/editor | **OPEN / CONFLICT** | Independencia conceptual vs acoplamiento del editor antiguo sin reconciliar. | Debate UX separado. |
| Python Project 3.14.7 vs metadata `==3.14.2` | **OPEN / SEPARATE** | Diferencia transversal real, no introducir cambio masivo durante Alarm Adoption. | Incremento de distribución/configuración ajeno. |
| CI/checkout limpio y E2E tras commit final | **UNVERIFIED** | PASS pertenecen a working trees locales previos al push; se confirmó presencia de código en HEAD, no rerun en checkout aislado. | Gate posterior cuando corresponda. |

## Una única frontera siguiente

**B2c.1 — revisión/diseño y ejecución segura de las disposiciones B1 que el executor no cubre.** Abrir nuevo chat con un nuevo HEAD leído de Git, contrastar `adoption.py`, `adoption_execution.py`, tests B1, grupos y `reconcile_group_configuration`; acordar primero semántica y pruebas para source/target definido/ejecutable/vacío. No mutar código ni añadir nuevos contratos sin autorización. B2c.2 (commit global V1/V2 y conexión EFFECTIVE), integración de sources y Live quedan posteriores.

Los presentes reemplazos son locales; verificar diff contra el canonical real antes de copiarlos. No se realizaron mutaciones Git en este cierre.
