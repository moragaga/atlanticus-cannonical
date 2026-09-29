# ADA Command Center — Engine and Projections

Estado: **CURRENT — Source v3, B.2 READY local, WAL/EFFECTIVE, Engine CURRENT v1/FACTS v2, Delivery input CURRENT+FACTS; C2 identidad/configuración unificadas CLOSED. Live, Management y Analytics PLANNED**. Implementación actual leída en `atlanticus:main@18029e19ff01e58b9c9399c132ff32b5ca913f06`.

## Source y materialización exacta

`AlarmConfigurationSnapshot(configuration, ToolDependencyManifest(Cn))` queda congelado al publicar Source Rn. La proyección de entrada `ProjectionRecord[AlarmConfigurationSnapshot]` conserva contenido y procedencia; el resolver B.2 NO relee latest Tool Catalog para reinterpretar Rn/Cn.

```text
Alarm Source Rn / Tool manifest Cn
  -> Alarm Projection Cosmos (acquisition)
  -> qualification externa + resolver puro B.2
  -> VOLUMEN_PATH/ada-command-center/alarms/materialization/
       ready.json
       versions/<result_id>/manifest.json
       versions/<result_id>/runtime.json
       versions/<result_id>/delivery.json
```

READY se publica únicamente con pareja Runtime/Delivery íntegra y coincidente. BLOCKED mantiene diagnóstico sin artefactos ejecutables ni reemplazar READY. La antigua salida monolítica B.2 a Cosmos está SUPERSEDED; no introducir salida doble ni adapters legacy.

## Delta C2

El valor acordado `APPLICATION=ada-command-center` figura en los tres `.env.detail` y manifiestos; los `JobDefinition` siguen siendo `alarms-materialization`, `alarms-runtime` y `alarms-delivery` con `job_key` propios, por lo que sus leases son independientes bajo raíz operacional compartida. `VOLUMEN_PATH` es absoluta, suministrada manualmente y debe señalar el **mismo montaje físico** para tres procesos. La raíz durable `VOLUMEN_PATH/ada-command-center/alarms` no se traslada ni se deriva de APPLICATION por el cambio.

Domain declara Source Key única `alarm-configuration`, consumida por Web y los tres jobs. Materialization deriva nombre físico de entrada y partición del existente `ALARM_CONFIGURATION_PROJECTION_STORAGE_RESOURCE`, sin env de nombre Cosmos. Sus endpoint/base/credencial siguen siendo explícitos; su identidad física con Web no se acreditó en recursos reales. No se agregaron migraciones: el usuario confirmó ausencia de despliegues previos que preservar.

## Runtime y outputs CURRENT

Runtime selecciona pin exacto `(source_key, result_id, manifest_sha256, resolution_key)` y verifica EFFECTIVE derivado del WAL. La adopción WAL V1/V2 y recuperación son contratos previos preservados; READY por sí solo NO implica EFFECTIVE.

Engine publica CURRENT v1 completo/reemplazable en `runtime/output/current/latest.json`, después de confirmar durabilidad requerida. Puede actualizar evidence aunque no exista un nuevo commit de ciclo de vida. Exporta de los commits durables FACTS v2 inmutables en `runtime/output/facts/`, encadenados con `previous_batch`; su progreso productor está en `runtime/output/state/facts-export-cursor.json`. Un FACTS v1 real heredado no obtiene adaptador por inferencia.

## Delivery input **actual**, no el objetivo C4

`processes/alarms-delivery` recibe **CURRENT y FACTS v2** desde Engine, valida checksum/source/pin y usa READY exacto de B.2 y EFFECTIVE de Runtime. Mantiene inbox y cursor independiente: `delivery/input/state/facts-consumption-cursor.json`. La configuración aún contiene `ALARM_DELIVERY_MAX_FACTS_PER_ITERATION` y su consumo de lotes; C2 no modificó esa conducta. El receptor no procesa el WAL ni constituye `AlarmLiveProjection`.

**C4 PLANNED (no ejecutado en este cierre):** retirar recepción de backlog FACTS en Delivery y consumir únicamente el último CURRENT. Mantener intactos FACTS v2 producidos por Engine para History/Analytics futuros. Antes de editar hay que auditar consumidores, pruebas e invariantes de recuperación; no inferir eliminación de información histórica ni construir Live durante C4.

## Fronteras separadas

- **Engine CURRENT:** verdad operacional serializada de su ciclo.
- **Delivery input CURRENT:** recepción y validación, no enriquecimiento Live.
- **AlarmLiveProjection:** contrato futuro de enriquecimiento por Delivery Configuration exacta, visibilidad/prioridad ya resuelta y `cause_text`, no implementado.
- **Management Projection / Capture:** historia/acciones de usuarios, responsabilidad distinta.
- **History/Analytics:** modelo de lectura futuro desde hechos durables; jamás Web leyendo WAL como API.

La prueba B2c.7 histórica demostró integración controlada y reinstanciación, no Docker aislado ni multi-host. C2 aportó pruebas locales de configuración y regresiones acotadas, **no** reconvalidó todo ese gate físicamente. El test del catálogo operacional fue corregido por el usuario en el commit C2 final, sin evidencia de ejecución total posterior compartida en este cierre.

## Próxima frontera sugerida, sin autorización de implementación

Debatir **C4 Delivery CURRENT-only** exclusivamente. C3 producer qualification, C5 evidencia técnica, Docker y Live/Management/History permanecen fuera del incremento.
