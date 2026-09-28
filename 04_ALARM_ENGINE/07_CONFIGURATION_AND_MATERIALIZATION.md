# Alarm Engine — Configuration and Materialization

Estado: **CURRENT — Source v3, B.2 READY/BLOCKED local, pin y adopción exacta; Runtime/Delivery consumen artefacto exacto; infraestructura física UNVERIFIED**. Corte 2026-09-28. **Código del hito verificado por lectura remota en** `atlanticus@c67fcb5b105cc561c16719a8bca4ea5aa74c3fae`; `main@bc1d73742bcb04eb495bbbb1725a8ad23d4eff38` está un commit posterior con cambios sólo de ADA Generic Master Projection, fuera de este alcance. Los gates locales son evidencia del usuario, no CI de este checkout.

## Invariantes de entrada

- `AlarmConfiguration` editable: `rules`/`messages`, `AlarmIdentity(family_key,alarm_key)` estable; family no se inventa como aggregate persistido adicional.
- `AlarmConfigurationSnapshot` Source schema **v3** congela Rn y `ToolDependencyManifest(Cn)` exacto. Source v2 SUPERSEDED, sin decoder legacy.
- Save/Validate/Publish respetan pin Cn, drift deliberado y referencias Tool; `VALID_AT_SAVE != READY != EFFECTIVE`; `INVALID != REMOVED`, `DISABLED != REMOVED`, `TRACE_ONLY != REMOVED`.
- `resolve_alarm_configuration` es puro; **el paquete** materialization incluye además codecs, reader e I/O. Cosmos puede ser entrada ProjectionRecord; la salida B.2 ya es **local READY/BLOCKED**, no Cosmos dual.

## Cadena exacta CURRENT

```text
Source/Projection Rn-Cn -> adquisición/qualification -> resolver B.2
  -> READY inmutable (manifest + runtime.json + delivery.json) / BLOCKED findings
  -> lector READY para candidato o lector exacto por result_id+manifest_sha256
  -> B1 AlarmConfigurationArtifactRef/Revision -> B2a adoption global WAL V1/V2
  -> B2b EFFECTIVE derivado del WAL -> Runtime job/sesión fijada
  -> Engine CURRENT v1 + FACTS v2 encadenados
  -> Delivery input receiver reabre la misma Delivery Configuration exacta
```

`AlarmResolutionKey(Rn,Cn)` identifica resolución lógica pero **no** resultado físico de qualification; el pin añade `source_key`, `result_id` y manifest SHA. Cambios sólo Delivery también pueden exigir adopción durable, incluso sin group commit. El job fijado no reinterpreta latest READY en cada ciclo. `effective-head.json` está bajo runtime/state, no Materialization.

## B2c.5c: fuentes/requisitos conservados

`AlarmEvaluatorContract` acepta requisitos estáticos `tuple[DataRequirement,...]` o un `requirements_resolver` alternativo; el plan consolida vistas pero cada alarma recibe sólo lo declarado. `source_reader.py` y adapter integran aplicaciones/fuentes/particiones registradas con `DatasetRuntime`/Parquet, sin confundir error de lectura/schema con INACTIVE. El entorno controlado no demuestra todos los datasets físicos.

## B2c.5d: catálogo productivo y ejemplo

La lógica por `(family_key,evaluator_key)` declara manualmente requisitos de datos; sus parámetros de negocio Web son opcionales y no dirigen automáticamente fuentes/columnas/particiones. El puerto genérico `requirements_resolver` no se elimina. `catalog/registry.py` permanece con registro productivo vacío; `catalog/examples/threshold` contiene el ejemplo explícito de test `mina.threshold`: PI_INTERPOLATED/DAILY, `temperature` FLOAT/4h, `limit` opcional 80.0, `EvidenceSnapshot` JSON-compatible o ERROR. No generar lógicas productivas desde un ejemplo.

## B2c.7: vinculación con publicación y Delivery

El Engine produce `current/latest.json` v1 y FACTS runtime v2 sólo bajo sesión EFFECTIVE confirmada. El receiver de `processes/alarms-delivery` valida el pin de la salida contra la **proyección** EFFECTIVE y abre `delivery.json` con `LocalAlarmMaterializationReader.read_exact_ready`. Una mismatch provoca espera/rechazo, nunca fallback a READY más reciente ni reinterpretación de Rn/Cn. `alarms/contracts/*.schema.json` son definiciones estáticas, no JSON emitidos/sobrescritos en ejecución.

## Frontera siguiente única, exclusiones

**PLANNED:** verificar empaquetado/distribución y ejecutar Engine y Delivery independientemente en Docker. No volver a diseñar Source/B.2, no registrar evaluadores de ejemplo, no cambiar Web/fin del turno, no crear otro journal ni interpretar la publicación de FACTS como History/Live ya implementado. Los volúmenes v1 preexistentes requieren inventario/decisión antes de intentar v2.
