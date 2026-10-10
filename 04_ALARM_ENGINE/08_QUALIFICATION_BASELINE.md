# Alarm Engine — Qualification Baseline

Estado: **CURRENT — suites locales y stress sintético del nuevo Runtime VERIFIED; qualification productiva end-to-end UNVERIFIED**. Corte: 2026-10-10.

## Baseline de implementación actual

```text
implementation: moragaga/atlanticus@758249d5fa35236b0ac9b990a393083b4463a507
canonical revisado: moragaga/atlanticus-cannonical@e950e2d0e2817ba25789765e9dbb0ef790866af3
fecha de evidencia local: 2026-10-08
```

**VERIFIED mediante logs aportados por el usuario:** suite `scopes/ada-alarm-engine` con **471 PASS**, Ruff check PASS, Ruff format PASS, `uv lock --check` PASS y `git diff --check` PASS. El resultado corresponde a una ejecución local reportada, no a CI remota ni a una ejecución reproducida por este documento.

**VERIFIED por revisión de fuentes/tests:** existen escenarios de recuperación de WAL, caída antes/después de Durable Head, materialización parcial de V2, takeover/fencing, recovery exacto del artifact EFFECTIVE, commits de ciclos, incidentes, rebases y adopción con cierre operacional. 13F.2c.3d cerró la auditoría sin introducir tests duplicados.

## Qualification del incremento 2026-10-10

**VERIFIED en `atlanticus:main`:** contratos FACTS v4, política de resúmenes en backend/runtime, WAL rotation/checkpoint/compaction y publicaciones CURRENT/FACTS del nuevo Runtime, en cuatro commits:

```text
b0c3a3d98a5c78a51321272bb3b0c7bc30494952  contracts FACTS v4
0fdf8f30  backend/runtime iteration summary
f48c9dca  alarm persistence WAL/recovery
c3b8ed3b8de4bbafdaeeff4410d4daaa20bed1b4  Alarm Runtime publication/stress
```

**VERIFIED por logs locales aportados por el usuario:** 70 PASS `ada-contracts-alarms`, 147 PASS `backend/runtime`, 171 PASS `alarms/persistence` y 165 PASS `alarm-runtime` incluyendo `stress/tests`: **553 PASS en cuatro suites**. Ruff check PASS en archivos involucrados y formato aplicado; no se afirma CI de todo el monorepo.

**VERIFIED por readjudicación local reportada de una ejecución sintética anterior de diez minutos:** escenario de tres alarmas y Acceptance Gate 7/7 (`runtime`, `scenario`, `recovery`, `head`, `rotation_compaction`, `facts_publication`, `current`); checkpoint sequence 10, un segmento WAL retenido, 322 batches FACTS y dos grupos CURRENT. **Límite:** no hubo una nueva corrida física del harness actualizado posterior al gate; su veredicto se obtuvo readjudicando evidencia persistida. Tampoco equivale a un evaluador productivo con datos reales, CI o despliegue Azure.

## Qualification física histórica — no transferible

Baseline anterior de Command Center:

```text
implementation histórica: atlanticus@38379979fad90e2c514a2d56f3aa3889ceb71856
canonical histórica: atlanticus-cannonical@8efd59431754059c548ed1e5d1263533b81012cd
```

La qualification local histórica registró:

```text
Cosmos database reachable
alarm-configuration container created/used
alarm-live-projection container created/used
Materialization READY produced
Runtime exact artifact adopted EFFECTIVE
NOTPII daily dataset consumed
Runtime alarm evaluated ACTIVE
priority disposition PREDOMINANT
Tool assignment present
CURRENT + FACTS written
Modeler index + Tool snapshot written
Delivery CURRENT_AVAILABLE
published_documents = 1
failed_tools = 0
Cosmos read-back matched modeled snapshot
```

Es evidencia **HISTORICAL / VERIFIED local en su propia generación**. No demuestra que el nuevo `scopes/ada-alarm-engine/processes/alarm-runtime` publique CURRENT/FACTS, ejecute evaluadores reales o complete un pipeline físico equivalente.

## Evaluadores y qualification producer

El ejemplo `mina/threshold` utilizado en la qualification histórica es example-only. En la composición productiva actual, `build_alarm_evaluator_registry()` retorna `AlarmEvaluatorRegistry(contracts=())`. La inyección controlada de evaluadores en tests no equivale a habilitar un catálogo productivo.

El `qualification.json` histórico se preparó localmente para el E2E de su generación. No constituye evidencia del productor productivo definitivo de qualification.

## UNVERIFIED — fuera del cierre 13F.2c

- Arranque físico productivo de `ada-alarm-engine` con evaluadores registrados y datos de entrada representativos (el estrés sintético del nuevo proceso no prueba esto).
- Continuidad física desde el nuevo CURRENT durable/FACTS v4 hacia consumidores Modeler/Delivery, sin equiparar formatos históricos.
- CI remota, dos contenedores concurrentes, Azure/Cosmos productivos, Key Vault y Entra.
- Recovery de scheduler avanzado, renderizado Web y proyecciones Management/Analytics.

Las ausencias anteriores son límites de qualification o frentes separados: **no reabren por sí mismas los contratos ya verificados de WAL, recovery y adopción**.
