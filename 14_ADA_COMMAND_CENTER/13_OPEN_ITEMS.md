# ADA Command Center — Open Items

Estado: **CURRENT — C1 extracción de Tool UI y traslado de Tool services a Web CLOSED estructuralmente; C2/C3/C4/C5 PLANNED; Starter, Azure, UX y otros frentes OPEN**. Corte C1 `atlanticus:main@3961385aecd0eb7e373018fc25e509a71dccc409`, decisions `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`, canonical base `faec587c3fb321e76c1a3da38a4d8193a2f1fdb5`. Los checkpoints históricos de Engine/B1d conservan fechas y alcances propios.

## Estado del frente Web después de C1 (2026-09-29)

| Elemento | Estado | Condición / evidencia delimitada |
|---|---|---|
| Publicación/descubrimiento/consolidación Tool Catalog | CURRENT / qualification B1d local CLOSED | Dos Sources/Projections de qualification, discovery READY y revisión Blob local; no Azure ni Starter. |
| Biblioteca `web/tools/catalog` | CURRENT / C1 CLOSED | Ya aloja snapshots/consolidación/Blob con distribución nueva; viejo `backend/tools/catalog` SUPERSEDED. |
| Biblioteca `web/tools/discovery-cosmos` | CURRENT / C1 CLOSED | Ya aloja conexiones nombradas y servicio inspect/confirm/adopted; viejo `backend/tools/discovery-cosmos` SUPERSEDED. |
| `web/tools/catalog-manager` | CURRENT / extracción UI CLOSED | UI/callbacks independientes; Configuration Manager la compone sin implementación duplicada. |
| Host temporal `ada-command-center-configuration-manager` | CURRENT | Regresiones/importaciones locales tras C1 PASS; browser/aceptación final post-C1 UNVERIFIED. |
| Starter `ada-command-center-generic` | PLANNED | Componer/distribuir capacidades Web independientes, sin dependencia de ADA Generic ni paquetes prematuros de identidad/navigation. |
| Barrido `.env.detail` y configuración de procesos Alarm | PLANNED C2/C5 | Faltan reglas comunes APPLICATION/SourceKey y nombre Cosmos físico por contrato, más auditoría variable por variable; conservar contenedor Blob ambiental. |
| Qualification GREEN/Evaluator automática de Materialization | PLANNED C3 / BLOCKED BY DESIGN | No productor/verificadores reales confirmados; hoy JSON manual de input. |
| Delivery último CURRENT y limpieza del arranque | PLANNED C4 | Receptor actual CURRENT+FACTS y cursor; sin replay en contrato futuro, Runtime FACTS permanece. |
| Evidencia técnica Runtime KEY/VERSION | PLANNED C5 / OPEN | Eliminar parametrización no útil sólo al identificar key/version y propietario real; no inventar constantes. |
| Instalación aislada wheels / CI / Azure / browser Web | UNVERIFIED | C1 validó wheel build/importaciones, no estos gates. |
| UX Tool Catalog | OPEN / SEPARATE | Revisión visual y convenciones Manager; no tests de CSS visual. |
| Desactivación fin del turno y cierre del modal | OPEN / SEPARATE | Requiere semántica/UX explícitas, no corregidas por C1. |
| Alarm Source Blob y Projection Cosmos durable | UNVERIFIED / SEPARATE | B1d sólo demostró Source local; no inferir durable E2E ni migración automática. |

C1 CLOSED no implica Golden Path Alarm durable completo ni Starter distribuible funcional. El directorio de qualification Tool B1d temporal fue limpiado tras ese corte; no reconstruirlo por inferencia.

## Contratos CURRENT/CLOSED preservados

- Domain Alarm extraído y Tool Catalog v1, autoría estructurada Rules/Messages, `ToolDependencyManifest` con revisión exacta, Source schema v3, guard de drift en Validate/Publish y referencias Tool congeladas Rn/Cn.
- Resolver B.2 puro; Materialization local READY/BLOCKED y lector exacto; Engine WAL B1/B2a/B2b, EFFECTIVE y Runtime B2c; outputs CURRENT v1 y FACTS v2, receptor Delivery actual.
- Gates históricos específicos B2c.5c informaron 436 PASS; B2c.5d 7 específicas/443 regresión PASS y Ruff. Estos conteos NO son C1 ni certificación Azure.
- Gates C1 reportados: pruebas de catálogo, discovery, UI y consumidores, Ruff/format, dos wheels y smoke importación host. No atribuir a C1 distribución instalada aislada o pruebas browser.

## OPEN — fin del turno y modal (anterior a C1)

En Web Alarm, `default_deactivation.max_duration_hours` se presenta con `_number_field`; `AlarmDeactivationDefinition` y override de mensajes modelan `max_duration_hours: int|None`, y desactivación habilitada exige entero **1..12**. El Engine maneja timestamps UTC (`effective_until`) para solicitudes/efectos, pero ello no define la expresión «hasta fin del turno» como opción elegible. Pendiente decidir calendario operacional, zona Mine/Plant, inicio/límites, aprobación y overrides antes de modificar Domain/Source/Engine/UI. No añadir enums ni conversiones por intuición. Modal: cierre sólo tras guardar exitosamente, evaluación UI separada. Consultar `18_ALARM_AUTHORING_UX_AND_VISUAL_PRESENTATION.md`.

## HISTORICAL — B2c.6 y B2c.7

El corte B2c.6 solicitó auditar la composición ejecutable de Runtime: `build_alarm_runtime_process` recibe registry y source_loader; `build_alarm_source_adapter` existe. `catalog/registry.py` productivo permanece vacío y `catalog/examples/threshold` es muestra, no evaluador autorizado. Las salidas/current/FACTS y el input receiver se incorporaron posteriormente en B2c.7 y están presentes antes de C1. La qualification Docker independiente de Engine/Delivery es otro gate todavía UNVERIFIED; no desplaza C2 como próximo foco aquí.

## Otros OPEN no iniciados durante C1

- **Web authoring UX/host/browser:** ayudas, gestión de familia, persistencia, feedback y flujos Source/Projection físicos fuera de gates automáticos locales.
- **UI Tool revision:** notificación explícita de Cn cambiado frente al pin guardado, sujeta a aceptación.
- **Routing visual:** contrato conceptual visual targets independiente del routing vs sincronización implementada del editor, conflicto pendiente.
- **Cosmos Alarm physical topology:** Manager ya deriva contenedor de resource contract; Materialization aún lo recibe desde env (C2).
- **Qualification operativa:** sin productor GREEN/evaluator real; no autodeclarar éxito ni confundir archivo previo con artefacto READY (C3).
- **Delivery/Live:** receiver actual no constituye Live Projection; pendiente C4 y contrato futuro Live por separado.
- **Management Capture/Projection, History/Analytics:** PLANNED/SEPARATE, Web no lee WAL.
- **Source v2 en despliegues reales:** sin decoder legacy actual; revisar sólo si existe estado real que requiera migración.
- **Python objetivo:** 3.14.7 del Project contra metadata `==3.14.2` en paquetes Command Center; scope de distribución ajeno a C1/C2 salvo acuerdo.
- **OperationalScope semana para PI/KPI:** no confundir otros enums/capacidades, frente separado.
- **Boundary de infraestructura:** Materialization todavía importa `web/alarms/projection-cosmos`; diseñar owner cuando corresponda, no trasladar automáticamente a Domain puro.

## Siguiente foco único

C2: leer de nuevo `atlanticus:main` y las decisiones/canónicos vigentes, fijar el contrato mínimo compartido de APPLICATION/VOLUMEN_PATH/Source Key y resolver Cosmos physical name de Materialization desde contrato existente. Mantener nombre de contenedor Blob en `.env`. Producir sólo propuesta primero; implementar más tarde tras autorización. No reabrir C3/C4/C5/Starter/UX en el mismo incremento.
