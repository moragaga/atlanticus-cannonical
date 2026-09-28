# Alarm Engine — Open Items

Estado: **B2c.5c y B2c.5d CLOSED en evidencia local e integrados en `atlanticus:main`; B2c.6 PLANNED; infraestructura física UNVERIFIED**. Corte 2026-09-28: código `a799dc15105d3e037f36ab77129ef0cfa8999013`, decisions `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`, canonical previo `46877f174513b2475f17b7dc739cd43951fa4ed0`.

## CURRENT / CLOSED en el alcance demostrado

- Source v3 Rn/Cn y Tool manifest exacto; B.2, qualification controlada, READY/BLOCKED local; lector compartido READY/exacto, sin salida Cosmos dual.
- B1 referencia exacta y planificación sobre identidades definidas; B2a WAL V1 sin grupos y V2 con 1..N grupos, ambos formatos CURRENT; B2b Effective Head reconstruible y selección de revisión exacta; B2c ejecutor/iteración/recuperación/sesión fijada presentes en main y probados localmente por los incrementos previos.
- B2c.5c: lector de fuentes/particiones registradas, carga consolidada y proyección de datos por alarma. Gate local reportado: 436 pruebas de regresión PASS y Ruff/format/diff sin hallazgos.
- B2c.5d: ejemplo controlado en `catalog/examples/threshold`; catálogo productivo deliberadamente **vacío**; requisitos del desarrollador estáticos e independientes de parámetros Web opcionales. Gate local reportado después del traslado: 7 específicas y 443 de regresión PASS, Ruff PASS, 40 archivos formateados. Confirmación remota de presencia del código en `a799dc1`.

## OPEN — razones y frontera de salida

| Elemento | Estado | Motivo / acción posterior |
|---|---|---|
| Composición ejecutable real `alarms-runtime` | **PLANNED — siguiente foco único B2c.6** | Existen `build_alarm_runtime_process` y `build_alarm_source_adapter`; no está validado el wiring operacional real con registry productivo, rutas de datasets, PI provider y dependencias de lanzamiento. Inspeccionar puertos y entrypoints existentes antes de diseñar pieza nueva. |
| Catálogo productivo real | **CURRENT vacío / PLANNED** | Cero evaluadores reales registrados. El ejemplo no debe incorporarse productivamente. Registrar futuras lógicas sólo cuando el usuario autorice contratos efectivos. |
| Pruebas físicas de todas las fuentes/particiones | **UNVERIFIED / condicionado por entorno** | El usuario aún no dispone de dataset preparado ni ambiente montado. Tests usan datos controlados; validar sólo cuando exista entorno. |
| Pipeline real Tool GREEN/evaluator qualification | **UNVERIFIED / OPEN** | Registro de evaluadores y qualification JSON controlada no implican productor/servicio real. |
| Deactivation Web hasta fin del turno | **OPEN / SEPARATE** | `default_deactivation.max_duration_hours` es numérico en Web; Domain exige int 1..12 para deactivation habilitada. La intención/efecto de Core llevan `effective_until` UTC, pero no hay política confirmada para fin de turno. Requiere debate Web/Domain/Core/calendario y tests de horario/cap sin introducir solución aquí. |
| Semana operacional genérica para PI/KPI Runtime | **OPEN / SEPARATE** | `OperationalScope` actual no ofrece semana genérica; `ShiftScope.CURRENT_WEEK`, `TimeWindow` y `DataPartition.WEEKLY` de FABRICA_PLANES no son sustitutos semánticos. Registrar para el frente KPI correspondiente, no implementar en Alarm Engine. |
| Política de cambio `evaluator_key`, `kind` y `priority_group` | **OPEN / CONFLICT Decisions vs main** | B.1 documenta compatibilidad/migración deseada, pero `adoption.py` sigue rechazando. Exige decisión/validación separadas. |
| Migración de prioridad con Rule disabled y estado residual | **OPEN / riesgo previo no resuelto** | No inferir búsqueda global o migración automática. La restricción operativa previa se conserva hasta verificación específica. |
| Reappearance de `ManagementEffect` vivo con cambios de timer/SC | **OPEN / SEPARATE** | Necesita reconciliación explícita y pruebas cuando se decida su alcance. |
| `resolution_key_at_start` de occurrence | **PLANNED / opcional pendiente de contrato** | No añadir dato persistente sin decisión de provenance. |
| Live Delivery / Management Capture / History-Analytics | **PLANNED / SEPARATE** | No leer WAL desde Web; proyectar hechos durables según contratos futuros. |
| Lecturas externas de snapshots durante adopción V2 | **OPEN para lectores futuros** | Materialized Head es barrera agrupada; raw reads multiarquivo no son MVCC. Diseñar al abordar Live/History. |
| Migración condicional desde histórico group-only | **OPEN / condicionado por despliegue** | Primer adoption sobre historial incompatible falla cerrado; no introducir migración ni legacy sin inventario real. |
| Visual targets vs routing del editor | **OPEN / CONFLICT UX** | Independencia deseada vs sincronización actual; no corregir incidentalmente. |
| Python 3.14.7 objetivo vs `==3.14.2` de paquetes | **OPEN / SEPARATE** | No forzar cambio transversal durante composición de alarmas. |
| CI limpia, builds después del traslado, Azure/Blob/Cosmos y volumen multi-host | **UNVERIFIED** | Logs locales anteriores al commit no son CI/E2E ni certifican wheels nuevos; requiere gate dedicado al tener condiciones. |

## Una frontera siguiente y exclusiones

**PROPOSED B2c.6:** debate sobre la composición ejecutable existente de `alarms-runtime`, sin alterar contratos de Domain/Core/Persistence/Materialization ni registrar ejemplos productivamente. Revisar `process.py`, entrypoints/scripts actuales, `catalog/registry.py`, `source_reader.py` y pruebas antes de escribir código. Solo después de consenso realizar wiring mínimo comprobable sin datasets reales. Deactivation al fin de turno, semana PI, Analytics, Live, qualification real y versiones/distribución se mantienen OPEN **pero fuera de este incremento**.

Los reemplazos MD son locales; comparar contra canonical HEAD antes de sobreescribir. No hay mutación Git autorizada en esta entrega.
