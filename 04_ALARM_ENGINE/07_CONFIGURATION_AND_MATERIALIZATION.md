# Alarm Engine — Configuration and Materialization

Estado: **CURRENT — Source v3, B.2, publicación local exacta, B1, B2a/B2b y vínculo B2c; integración física real UNVERIFIED**. Corte 2026-09-28: `atlanticus@a799dc15105d3e037f36ab77129ef0cfa8999013`.

## Invariantes de entrada y publicación

- `AlarmConfiguration` editable: `rules` y `messages`, identidad estable `AlarmIdentity(family_key,alarm_key)`; la familia se deriva y no tiene objeto persistido independiente.
- `AlarmConfigurationSnapshot` Source schema **v3** congela configuración Rn y `ToolDependencyManifest(Cn)` exacto. Source v2 está SUPERSEDED; no crear decoder legacy.
- Save Draft, Validate y Publish respetan pin Cn, drift deliberado y referencias Tool exactas. `VALID_AT_SAVE != READY != EFFECTIVE`; INVALID, DISABLED y REMOVED conservan semánticas distintas.
- `resolve_alarm_configuration` de `backend/alarms/materialization/resolver.py` es **puro**. El paquete contiene además codec y lector con I/O; no describirlo globalmente como puro.
- La proyección Cosmos es **entrada** de Materialization; el job publica en volumen local un resultado READY inmutable con pareja Runtime/Delivery exacta y manifest o un resultado BLOCKED diagnóstico. No hay salida dual Cosmos/local ni `effective.json` bajo Materialization.

## Cadena exacta CURRENT

```text
Projection/Source Rn-Cn
  -> acquirer + qualification + B.2 resolver
  -> READY local (manifest, runtime.json, delivery.json) / BLOCKED diagnóstico
  -> lector local READY o lector exacto por result_id + manifest_sha256
  -> AlarmConfigurationArtifactRef + AlarmConfigurationRevision
  -> AlarmConfigurationAdoptionExecutor (WAL V1/V2 y confirmación)
  -> AlarmEffectiveConfigurationHead derivado del WAL
  -> RuntimeLocalConfigurationReader exacto + sesión del job fijada
```

El token Rn/Cn identifica una resolución lógica; no identifica por sí solo un resultado de qualification: la adopción usa `source_key`, `result_id`, hash exacto del manifest y `resolution_key`. Los cambios Delivery-only también pueden exigir adopción durable sin cambio de grupo. El job pinnea la configuración efectiva y no reinterpreta latest READY en cada ciclo. Revalidar código si se propone modificar adopción, recovery o lectura exacta.

## B2c.5c — frontera de fuentes CURRENT

`processes/alarms-runtime/session.py` acepta `AlarmEvaluatorContract` con requisitos estáticos `tuple[DataRequirement,...]` o `requirements_resolver`, excluyentes. `source_adapter.py` y `source_reader.py` conectan el plan consolidado con el registro existente de fuentes, rutas de aplicación y `DatasetRuntime`/Parquet. Se entregan contextos por consumidor y los errores de fuente/esquema no deben confundirse con alarma INACTIVE. Existencia y pruebas con backend de datos controlado **no** demuestran carga real de todos los datasets.

## B2c.5d — catálogo CURRENT, ejecución de ejemplo CLOSED local

El contrato de ejecución se resuelve por `(family_key,evaluator_key)` y entrega `EvaluationContext` con parámetros de negocio opcionales. El desarrollador define manualmente los requisitos de sus nuevos evaluadores: **no** calcular fuentes/particiones/columnas desde parámetros de configuración Web en el ejemplo/convenio actual. Esto refina la primera propuesta B2c.5d y **no elimina** el puerto genérico existente `requirements_resolver`.

`catalog/registry.py` devuelve registro de producción vacío; el ejemplo está en `catalog/examples/threshold/`, separado de cualquier catálogo operacional real, con espejo comentado. El ejemplo define PI_INTERPOLATED/DAILY, temperatura FLOAT y ventana 4h; usa `limit` opcional con default demostrativo 80.0. No introducir parámetros sintéticos ni validador universal. Evalúa ACTIVE/INACTIVE o ERROR; EvidenceSnapshot es JSON-compatible y corresponde a la alarma/ocurrencia conforme a reglas de Core, no es un mensaje directo a Web.

## Frontera siguiente única

**PLANNED B2c.6:** comprobar y, sólo tras acuerdo, conectar la composición de proceso realmente existente con `AlarmEvaluatorRegistry` productivo vacío, `build_alarm_source_adapter`, rutas/PI provider/volumen y demás dependencias ya contratadas. No inventar bootstrap o cliente global; primero inspeccionar entrypoints, scripts, `process.py` y tests reales. Fuera de B2c.6: nueva política de desactivación hasta fin de turno, semana operacional PI, Live/Analytics, nuevas fuentes y despliegue físico.
