# Alarm Engine — Index

Estado: **CURRENT — Runtime durable, publicación CURRENT/FACTS v4 y maintenance WAL publicados; stress sintético local VERIFIED; integración productiva downstream UNVERIFIED**. Corte: 2026-10-10. Implementación: `atlanticus@c3b8ed3b8de4bbafdaeeff4410d4daaa20bed1b4`.

## Frontera actual de implementación

La implementación de referencia del Runtime de este hito está en:

```text
scopes/ada-alarm-engine/
  alarms/core/
  alarms/materialization/
  alarms/persistence/
  processes/alarm-materialization/
  processes/alarm-runtime/
```

El proceso `ada.processes.alarm_runtime` consume configuración READY, conserva la autoridad EFFECTIVE recuperable desde WAL, ejecuta ciclos y confirma cambios de lifecycle mediante commits durables bajo lease/fencing.

La integración de inputs del proceso actual utiliza `DataInputLoader` y `RoutedDatasetSourceReader` de Operational Data. El bloqueo histórico por imports de contratos legacy corresponde al antiguo `scopes/ada-command-center/backend/processes/alarms-runtime` y **no describe** el proceso `scopes/ada-alarm-engine/processes/alarm-runtime` actual.

## Estado por responsabilidad

| Frontera | Estado | Alcance de la afirmación |
|---|---|---|
| Domain, evaluación y lifecycle | CURRENT | Código y tests presentes en `ada-alarm-engine` |
| Materialization READY | CURRENT | Artifact exacto sujeto a verificación de identidad |
| Runtime durable, WAL, recovery y fencing | CURRENT | Incrementos 13F.2c.3a–13F.2c.3c.3 |
| Adopción READY → EFFECTIVE y reconciliación operacional | CURRENT | Bootstrap V1, adopción V2 y commits V3 |
| Auditoría de cobertura 13F.2c.3d | CLOSED | Sin brecha demostrada que exigiera tests duplicados |
| Stress sintético físico del **nuevo** proceso | VERIFIED local | Tres alarmas, 10 minutos, Acceptance Gate 7/7; no equivale a evaluadores productivos |
| Registry productivo de evaluadores | OPEN | `build_alarm_evaluator_registry()` retorna `contracts=()` |
| CURRENT durable v1 y FACTS stream/cursor v4 del nuevo Runtime | CURRENT | Publicadores integrados en composición; no son el contrato de Modeler histórico |
| Modeler, Delivery y Web | Separados | No incluidos en el cierre durable 13F.2c |

## Incremento de publicación y retención — 2026-10-10

**CURRENT por código:** el nuevo Runtime publica un CURRENT durable en `current/durable-latest.json` y un stream FACTS v4 en segmentos JSONL horarios bajo `facts/year=.../month=.../day=.../hour=.../part-....jsonl`, con cursor durable `state/facts-export-cursor.json`. FACTS y CURRENT son derivados del WAL, nunca autoridades de commits.

**CURRENT:** WAL con rotación, recovery checkpoints en dos slots y compactación con verificaciones de autoridad; `JobDefinition.iteration_summary_every=1` conserva el comportamiento de otros procesos, mientras Alarm Runtime utiliza política explícita de logging.

**VERIFIED local reportado:** 553 tests distribuidos en cuatro paquetes, Ruff validado por el usuario y estrés sintético v3 readjudicado con 7/7 controles.

**UNVERIFIED:** evaluadores productivos, equivalencia entre nuevo CURRENT durable y entrada de Modeler histórico, despliegue Azure/multi-host, y reducción suficiente del footprint total de facts.

## Ownership y contratos compartidos

`ada-contracts-alarms` mantiene los contratos compartidos de configuración/publicación. El núcleo generic de Atlanticus no depende de ADA. La ubicación y responsabilidades históricas de Command Center no se transfieren automáticamente a la nueva implementación.

## Persistencia y autoridad

```text
Materialization READY (candidata)
    ↓ qualification de ejecución y compatibilidad
WAL: commits de grupo + ConfigurationAdoptionRecord
    ↓ durable / materialized head y recovery
EFFECTIVE: proyección de autoridad exacta
    ↓ sesión y lifecycle fijados
Runtime: ciclos confirmados antes de publicar memoria
```

`READY != EFFECTIVE`. Ningún lector de Runtime puede usar el último READY como sustituto de EFFECTIVE tras un reinicio.

## Evidencia histórica y límites

El pipeline anterior en Command Center produjo evidencia física local de CURRENT/FACTS, Modeler, Delivery y Cosmos. Esa evidencia permanece **HISTORICAL** y no cualifica por inferencia la nueva composición `ada-alarm-engine`.

La frontera Web también permanece independiente: visibilidad de alarmas deriva del lifecycle/projection; Web no debe consumir WAL ni ejecutar su propio scheduler. El trabajo de scheduling avanzado de Modeler no se incluye en este cierre.

## Referencias

- `02_RUNTIME_AND_LIFECYCLE.md`
- `03_PERSISTENCE_AND_RECOVERY.md`
- `08_QUALIFICATION_BASELINE.md`
- `10_OPEN_ITEMS.md`
- `11_SOURCE_LEDGER.md`
- `13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md`
