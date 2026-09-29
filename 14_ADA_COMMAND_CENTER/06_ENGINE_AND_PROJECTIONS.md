# ADA Command Center — Engine and Projections

Estado: **CURRENT — Source v3; READY local; WAL/EFFECTIVE; Runtime CURRENT v1 + FACTS v2; Delivery input exclusivamente último CURRENT tras C4. C1/C2/C4 CLOSED local; Live, Management y Analytics PLANNED.** Código auditado `atlanticus@45eff96d777f4711cb011f779ffc0a6c87bf0ca4`.

## Source y materialización exacta

`AlarmConfigurationSnapshot(configuration, ToolDependencyManifest(Cn))` congela Rn/Cn. Materialization consume la proyección de entrada sin reinterpretar Tool latest; qualification manual externa y B.2 producen:

```text
Alarm Source Rn + Tool manifest Cn
  -> Alarm Projection Cosmos
  -> qualification externa (JSON manual CURRENT) + resolver B.2
  -> VOLUMEN_PATH/ada-command-center/alarms/materialization/
       ready.json
       versions/<result_id>/manifest.json
       versions/<result_id>/runtime.json
       versions/<result_id>/delivery.json
```

READY requiere pareja Runtime/Delivery íntegra, mismo pin, manifest/hash. BLOCKED deja diagnóstico sin sustituir READY. La salida monolítica previa B.2 a Cosmos es SUPERSEDED; no añadir otra vez ni adaptadores.

## C2 preservado

Los tres procesos usan `APPLICATION=ada-command-center` con `job_key`/leases individuales; `VOLUMEN_PATH` absoluta y manual debe apuntar al mismo volumen físico. Source Key única Domain `alarm-configuration`; Materialization deriva contenedor/partición Cosmos del resource contract existente. Los bindings físicos de Cosmos de Web/Materialization y los montajes reales siguen UNVERIFIED. No hay legado desplegado que migrar.

## Runtime y outputs CURRENT

Runtime selecciona `(source_key, result_id, manifest_sha256, resolution_key)` exacto, adopta en WAL y mantiene EFFECTIVE; READY por sí solo no es EFFECTIVE. Publica CURRENT v1 completo y reemplazable en `runtime/output/current/latest.json` tras confirmar durabilidad requerida. Puede cambiar evidence sin nuevo commit de ciclo de vida. Independientemente exporta FACTS v2 inmutables y encadenados por `previous_batch` bajo `runtime/output/facts/`, con cursor **productor** `runtime/output/state/facts-export-cursor.json`.

La recepción C4 no altera estos productos, el WAL ni la semántica de recuperación del Engine. No introducir FACTS v1 adapters.

## Delivery input — C4 CURRENT-only

`processes/alarms-delivery` consume **solo el último CURRENT**. Lee la proyección EFFECTIVE y valida el snapshot recibido (SHA256, source, formato, integridad, UTC, unicidad de occurrences y evaluación). Exige igualdad de pin exacto con EFFECTIVE y resolución pareja mediante `LocalAlarmMaterializationReader.read_exact_ready`. No usa latest READY como sustituto ni consulta el WAL directamente.

- Si no hay EFFECTIVE: `WAITING_EFFECTIVE`; si no hay CURRENT o su pin difiere: `WAITING_CURRENT`.
- Si se pierde igualdad durante el ciclo: `EFFECTIVE_CHANGED`; no se incorpora el snapshot no alineado.
- Si `as_of` retrocede respecto al inbox: `STALE_SOURCE`; mismo timestamp con contenido distinto falla cerrado; igualdad exacta produce `CURRENT_UNCHANGED`.
- Si es válido y nuevo: se reemplaza atómicamente `delivery/input/current/latest.json` bajo fence y se informa `CURRENT_STAGED`.
- Un CURRENT válido con `alarms=[]` representa cero ocurrencias abiertas, distinto de ausencia física de CURRENT. El receptor no recorre snapshots intermedios ni espera backlog histórico.
- `recover()` valida el CURRENT recibido previamente; desaparecieron la lectura de FACTS, su cursor consumidor y el ajuste `ALARM_DELIVERY_MAX_FACTS_PER_ITERATION`.

**Decisión C4 sobre desalineación:** si EFFECTIVE es B y el CURRENT observado corresponde a A, Delivery no acepta B hasta que el último CURRENT coincida; no agregar coordinación temporal, retries especiales ni reescritura del Runtime. El archivo de inbox A puede seguir presente durante espera, pero este receptor no realiza despacho. Una futura Live Projection debe aplicar otra vez la igualdad exacta al consumir el inbox; su implementación no está autorizada por C4.

El bootstrap conserva `config/connections.json`, creación de `ParallelCosmosPublisher` y `ALARM_DELIVERY_MAX_WORKERS` sin modificar su funcionamiento. Si falta registry, el entrypoint hoy termina antes de ejecutar el job; separar la evaluación de esta dependencia de cualquier futura simplificación de infraestructura.

## Fronteras independientes

- **Engine CURRENT:** estado operacional actual con prioridad ya resuelta.
- **Delivery input:** recepción/validación CURRENT y almacenamiento local, no publicación Live.
- **AlarmLiveProjection:** contrato futuro de enriquecimiento con Delivery Configuration exacta, visibilidad, causa y destino; NOT IMPLEMENTED.
- **Management Capture/Projection:** intenciones/acciones de usuarios; servicio y proyección distintos.
- **History/Analytics:** consumo futuro de hechos durables; Web no lee WAL ni deriva History del inbox CURRENT.

## Evidencia y siguiente frontera

**VERIFIED:** comparación Git entre baseline C2 y C4 (un commit, 14 cambios Delivery), 29 pruebas Delivery y 16 publicadores Runtime PASS comunicados; regresión local con workspace completo **567 PASS / 1 SKIPPED** y Ruff PASS en archivos C4. **UNVERIFIED:** CI, Docker separado y recursos físicos/multi-host; los gates históricos B2c.7 no equivalen a qualification distribuida.

**PROPOSED / PLANNED siguiente foco único:** revisar y calificar **artefactos distribuidos de Runtime + Delivery en Docker como procesos independientes**, con volumen compartido y configuración exacta. Sin abrir C3/C5/Live/History durante ese gate.
