# Alarm Engine — Index

Estado: **CURRENT — ADA Alarm Engine Runtime durable implementado; qualification física del nuevo proceso UNVERIFIED**. Corte: 2026-10-08. Implementación revisada: `atlanticus@758249d5fa35236b0ac9b990a393083b4463a507`.

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
| Ejecución física integral del **nuevo** proceso | UNVERIFIED | No sustituir con la qualification histórica |
| Registry productivo de evaluadores | OPEN | `build_alarm_evaluator_registry()` retorna `contracts=()` |
| Exportación CURRENT/FACTS del **nuevo** Runtime | UNVERIFIED / fuera de 13F.2c | La composición actual no conecta un publicador equivalente al legado |
| Modeler, Delivery y Web | Separados | No incluidos en el cierre durable 13F.2c |

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
