# ADA Command Center — Engine and Projections

Estado: **CURRENT Source v3, B.2 READY local, EFFECTIVE, Engine CURRENT v1/FACTS v2 e input receiver Delivery; Live/Management/Analytics PLANNED**. Corte: 2026-09-28. La descripción anterior según la cual la salida local o la adopción exacta son todavía PLANNED está SUPERSEDED en las piezas comprobadas.

## Configuration y evidencia exacta

`AlarmConfigurationSnapshot(configuration,ToolDependencyManifest(Cn))` se congela en Source Rn. La proyección de entrada `ProjectionRecord[AlarmConfigurationSnapshot]` conserva contenido/provenance sin usar latest Tool Catalog para reescribir Rn/Cn. Confirmed Tool Catalog dura en Storage según su dominio migrado; los accesos reales Azure permanecen UNVERIFIED en este gate.

## Materialization READY/BLOCKED CURRENT

```text
Alarm Source Rn / manifest Cn -> proyección de entrada Cosmos cuando corresponda
  -> acquirer/qualification + resolver B.2 puro
  -> VOLUMEN_PATH/ada-command-center/alarms/materialization/
       ready.json
       versions/<result_id>/{manifest.json,runtime.json,delivery.json}
```

READY se promueve sólo cuando la pareja exacta íntegra está publicada; BLOCKED conserva findings sin artefactos ejecutables ni desplazar READY legítimo. La salida monolítica Cosmos B.2 antigua es SUPERSEDED, sin adaptador temporal o salida doble. B.2 comparte `AlarmResolutionKey`, no confiere EFFECTIVE por sí sola.

## Runtime exacto y Engine CURRENT

B1 identifica `(source_key,result_id,manifest_sha256,resolution_key)`; B2a persiste adopción WAL V1 sin grupos/V2 con grupos; B2b proyecta EFFECTIVE recuperable desde WAL; B2c ejecuta la sesión y el ciclo con ese pin. Engine B2c.7a publica CURRENT v1 completo y reemplazable en `runtime/output/current/latest.json` tras confirmar los commits necesarios. La evaluación puede actualizar evidence aunque no haya lifecycle change. No leer WAL desde Live/Web como API de consulta.

## Engine FACTS v2 y Delivery input

Engine exporta sólo hechos confirmados del WAL a lotes FACTS v2 inmutables en `runtime/output/facts/`, ligados por `previous_batch` (ID+SHA). El cursor `runtime/output/state/facts-export-cursor.json` es progreso del productor. El job independiente `processes/alarms-delivery` recibe CURRENT y FACTS desde el volumen y conserva `delivery/input/state/facts-consumption-cursor.json` y copias validadas. Verifica identidad/configuración exacta, integridad y continuidad de cadena; no comparte el WAL ni su cursor.

La prueba de integración B2c.7c creó Engine y Delivery reales en entorno controlado y recreó instancias para comprobar recuperación; B2c.7d validó missing links, corrupción y recovery mediante 32 pruebas específicas. No se ensayaron dos contenedores Docker separados ni volumen multi-host. FACTS v1 runtime fue refinado a v2 **sin lector legacy**; los datos v1 reales deben tratarse como caso BLOCKED hasta decisión explícita.

## Proyecciones conceptualmente separadas

- **Engine CURRENT:** datos operacionales resueltos del ciclo, sin reglas visuales Web; CURRENT IMPLEMENTED.
- **Delivery input receiver:** validación/recepción y cursor independiente, CURRENT IMPLEMENTED.
- **AlarmLiveProjection:** futuro enriquecimiento de CURRENT con `DeliveryAlarmConfiguration` exacta, filtro visibility/priority y `cause_text`; CONTRACT AGREED pero NO IMPLEMENTED.
- **Management Projection/Capture:** responsabilidad y lifecycle propios; SEPARATE/PLANNED.
- **History/Analytics:** read model independiente basado en FACTS y futuros hechos de entrega; CANDIDATE/PLANNED, no inventar por existir archivos.

Web nunca calcula priority, reinterpreta `cause_template` ni consulta WAL. La publicación física futura de Live para Web, retention y hechos de despacho/escalamiento de Delivery requieren fronteras/contratos separados; no se abren durante qualification de distribución.

## Siguiente frontera única

**PLANNED:** comprobar paquetes/artefactos de distribución y ejecución Docker de Engine y Delivery como jobs independientes sobre un volumen compartido controlado, con configuración/entrypoints existentes. Identificar el test SKIPPED y la existencia/no existencia de histórico FACTS v1. No introducir nuevas clases, almacenes o migraciones sin auditoría y acuerdo.
