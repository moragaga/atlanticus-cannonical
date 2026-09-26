# Alarm Engine — Configuration and Materialization

Estado: **CURRENT / SOURCE V3 + PROJECTION STORES + PURE B.2 + STRICT ROUTING IMPLEMENTED / JOB PLANNED**

## Authority

```text
Implementation checkpoint: moragaga/atlanticus@411aea44ac60c09d2b07ce41d34c3f378788b97b
Canonical input: moragaga/atlanticus-cannonical@83cd871c8418e37d2c29dff30e2ea5ef54bda4a0
Historical decisions: moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

## Invariantes

```text
LATEST SAVED = LATEST VALID_AT_SAVE
VALID_AT_SAVE != READY != EFFECTIVE
INVALID != REMOVED
DISABLED != INVALID
DISABLED != REMOVED
TRACE_ONLY != REMOVED
READY != EFFECTIVE
```

`VALID_AT_SAVE` no implica qualification B.2 completa. Core, Web, pure resolver, job, artifact stores y Runtime Adoption son fronteras diferentes.

## Source y evidencia Tool exacta — CURRENT

```text
AlarmConfiguration(rules, messages)
AlarmConfigurationSnapshot(configuration, tool_dependencies: ToolDependencyManifest)
source document_type = ada_command_center_alarm_configuration_release
source schema_version = 3
```

V2 es **SUPERSEDED**, sin decoder de compatibilidad. Cada publicación Rn congela sólo las referencias Tool definidas (origen, todos los escalones habilitados o no, visual targets, Rules activas o no), reteniendo `ToolDependencyManifest(Cn)` con `display_name`, `source_release_id`, `kind` y `ToolStructure`. No adquirir el latest Tool Catalog para reinterpretar `Rn/Cn`.

El Manager Alarm conserva `_confirmed_tool_catalog_revision` como sidecar de draft; Validate y Publish exigen que coincida con la revisión confirmada, y publicación comprueba de nuevo el drift. La cualificación GREEN y del Evaluator corresponde a B.2, no al guardado local.

## Base/operational projection — CURRENT ADAPTERS; LIVE INTEGRATION UNVERIFIED

Owners existentes:

```text
web/alarms/configuration: source_release.py, source_projection.py, projection_record.py
web/alarms/projection-local
web/alarms/projection-cosmos
web/alarms/persistence: compose_alarm_configuration_persistence
```

`ProjectionRecord[AlarmConfigurationSnapshot]` conserva la release y el payload v3 íntegro. La composición acepta Source Local/Blob y Projection Local/Cosmos mediante settings explícitos. Cosmos store implementa `get_active` y `replace_active`. El host de pruebas configurado en main utiliza proveedores locales. **No inferir de la existencia de los adapters** que exista un productor Cosmos operativo, credenciales válidas ni integración Azure verificada.

## Pure B.2 resolver — CURRENT

Owner: `scopes/ada-command-center/backend/alarms/materialization`.

```text
AlarmConfiguration
+ alarm_configuration_revision
+ exact frozen Tool evidence (`ToolDependencyManifest` structurally)
+ ToolReconciliationQualification
+ EvaluatorQualificationCatalog
 -> resolve_alarm_configuration(...)
 -> AlarmConfigurationResolution(READY | BLOCKED)
```

Puro: sin I/O, stores, scheduler, acquisition, persistence ni Runtime Adoption.

Qualification:
- toda Rule definida necesita evaluator qualified por `(family_key, evaluator_key)`;
- toda Tool reference definida debe existir en la evidencia exacta y ser GREEN, incluidas Rules inactivas y steps deshabilitados;
- validar criticidad, dirección de routing y visual targets.

Hallazgos actuales, entre otros:

```text
evaluator_not_qualified
tool_reference_not_found
tool_reference_not_green
routing_invalid_for_criticality
routing_invalid_direction
visual_target_invalid
```

Atomicidad:

```text
READY   -> Runtime + Delivery (misma AlarmResolutionKey)
BLOCKED -> blocking finding + Runtime None + Delivery None
```

## Strict routing — FROZEN / IMPLEMENTED

`domain/alarms/routing_policy.py` define `next_routing_tool_kind`:

```text
PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC -> END
```

- Cada escalón habilitado avanza **exactamente** un nivel respecto del último escalón habilitado, en `step_order` ascendente.
- Nunca same-tier, retroceso ni salto directo `PROCESS -> STRATEGIC`.
- Strategic es terminal.
- Una Rule C1 o C2 puede tener **cero destinos**; no cambiar su criticidad automáticamente. C3 sólo admite origen y bloquea steps habilitados.
- C1 habilitado: inmediato (`None` o `0` en el campo wait).
- C2 habilitado: espera entera positiva **desde el paso anterior**; B.2 suma las esperas y entrega offsets absolutos desde `occurrence.started_at`. Dos waits de `20` dan due times a `20` y `40` minutos.
- Los pasos deshabilitados no participan en la ruta ni añaden tiempo, pero sus referencias Tool definidas siguen en el manifest y en qualification.
- Si falta una Tool de la evidencia exacta, emitir el finding de referencia; no inventar kind ni realizar lookup latest.

El formulario Web comparte la política para ofrecer sólo el siguiente nivel. Conserva selecciones antiguas incompatibles visibles para corrección, sin borrarlas. Strategic participa en `routing_tools`, pero no en el catálogo de visual targets porque su representación Alarm no está contratada.

## B.2 Materialization Process — PLANNED, SIGUIENTE FRONTERA

Path objetivo **histórico/propuesto en canonical**, aún ausente en `411aea...`:

```text
scopes/ada-command-center/backend/processes/alarms-materialization
```

No crear nuevo contrato hasta verificar los existentes. El siguiente chat debe contrastar: proveedor y lectura de `ProjectionRecord[AlarmConfigurationSnapshot]` operacional; exact revision/Source provenance; obtención real de Tool GREEN y evaluator qualification; fronteras de persistencia/publicación de Runtime, Delivery y findings; reintento/diagnóstico y ownership de settings. Sin esos datos, rutas físicas, intervalos, formatos de descarga, IDs y contenedores siguen **UNVERIFIED / OPEN**.

No reabrir Tool Catalog histórico ni recalcular snapshot v3. No añadir Runtime Adoption, Effective Head ni scheduler de Live Delivery al mismo incremento.
