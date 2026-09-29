# ADA Command Center — Configuration Scope

Estado: **CURRENT — Alarm Configuration Snapshot Source v3 / Tool dependencies Rn/Cn; C1 Web ownership y C2 identidad compartida implementadas. C4 Delivery CURRENT-only CLOSED en otro owner, sin alteraciones Source/Tool. Durable E2E y UX final UNVERIFIED.** Checkpoint `atlanticus@45eff96d777f4711cb011f779ffc0a6c87bf0ca4`.

Command Center administra Alarm Configuration reutilizando `atlanticus.web.manager` sin modificar semántica de Manager genérico.

## Aggregate y publicación CURRENT

```text
AlarmConfiguration
    rules
    messages

AlarmConfigurationSnapshot
    configuration: AlarmConfiguration
    tool_dependencies: ToolDependencyManifest
    schema_version: 3
```

Definiciones Tool no se embeben en configuración Alarm editable. El workspace tiene sidecar `_confirmed_tool_catalog_revision` no miembro de `AlarmConfiguration`. Source schema v2 SUPERSEDED sin decoder legacy.

### Flujo vigente

```text
Save Draft -> consulta Confirmed Tool Catalog y fija Cn en workspace
Validate   -> revisa configuración/Cn actual y referencias Tool
Verify     -> concurrencia de Source bajo Manager
Publish    -> vuelve a comprobar Cn (drift guard) y congela manifest Cn
```

El manifest incluye origins, todos los escalones de routing definidos —incluso inactivos— y visual targets de todas las Rules, incluidas inactivas. Cambios Cn posteriores no reinterpretan snapshots Rn/Cn inmutables. `VALID_AT_SAVE != READY != EFFECTIVE`.

## C1 — Tool ownership CURRENT

`web/tools/catalog` construye/persiste Confirmed Tool Catalog CURRENT en Blob; `web/tools/discovery-cosmos` inspecciona conexiones Tool Cosmos nombradas y confirma revisiones; `web/tools/catalog-manager` es dueño de UI/callbacks. Host temporal compone services; `backend/tools` SUPERSEDED. STRATEGIC puede recibir routing pero no visual target mientras no exista proyección visual acordada. El editor no reinterpreta snapshots congelados con Tool latest.

## C2 — Source Key y topología CURRENT

`domain/alarms/identity.py` define el texto `ALARM_CONFIGURATION_SOURCE_KEY = 'alarm-configuration'`; Web lo transforma en `SourceKey` técnico, y Materialization/Runtime/Delivery lo importan. La constante no suprime verificaciones de `source_key` en proyecciones, READY/EFFECTIVE y recepciones.

El resource contract Cosmos existente reside en `web/alarms/projection-cosmos/storage.py`:

```text
logical_id        ada.command_center.alarms.configuration.projection
physical_name     ada-command-center-alarm-configuration-projection
partition_key     /partition_key
allowed_override  CONNECTION_REF
```

Web lo resuelve al componer conexión. Materialization reutiliza nombre/partición, quitando env duplicada `ALARM_PROJECTION_CONTAINER`. Cuenta/base/credencial Cosmos de **entrada** Materialization siguen siendo manuales y deben coincidir físicamente con el host durable: aún UNVERIFIED.

El contenedor Blob permanece ambiental. Las conexiones de **salida** Delivery son independientes mediante registry nombrado; C4 no modificó esa infraestructura. `APPLICATION=ada-command-center` es común; `VOLUMEN_PATH` absoluta/compartida la define el operador, no se deriva de Source.

## Evidencia histórica y límites

B1d observó Confirmed Tool Catalog de dos Tool Sources/Projections controladas y Alarm Source local. C1 ownership Web CLOSED; C2 cerró divergencias de identidad/recursos contractuales. C4 comprobó por Git que sus 14 cambios están exclusivamente bajo Delivery; su regresión local 567 PASS/1 SKIPPED no certifica Alarm Source Blob + Cosmos E2E, Azure ni equivalencia física de montajes. Tampoco acredita browser final.

## OPEN y frentes distintos

- **C3:** productor/verificadores GREEN de qualification reales; `ALARM_QUALIFICATIONS_FILE` manual sigue CURRENT.
- **C4 CLOSED:** solo recepción último CURRENT con pin exacto; no altera authoring Source/Tool, qualification ni distribución física.
- **C5:** contract key/version de technical evidence Runtime y auditoría `.env.detail`, sin inventar valores.
- **Docker:** qualification distribuida Runtime/Delivery recomendada como próximo foco, sin alterar este aggregate.
- **UX:** fin de turno, modal de guardado y autonomía visual frente a routing requieren decisión/validación propia.

No usar este cierre documental para modificar código, UI o nuevos contratos de Source.
