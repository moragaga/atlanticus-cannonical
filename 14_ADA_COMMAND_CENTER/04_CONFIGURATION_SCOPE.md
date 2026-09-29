# ADA Command Center — Configuration Scope

Estado: **CURRENT — Alarm Configuration Snapshot Source v3, Tool dependencies Rn/Cn, C1 Web ownership y C2 identidad compartida implementadas; durable E2E y UX final UNVERIFIED**. Checkpoint: `atlanticus:main@18029e19ff01e58b9c9399c132ff32b5ca913f06`.

Command Center administra Alarm Configuration y reutiliza `atlanticus.web.manager` sin modificar la semántica del Manager genérico.

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

Las definiciones Tool no se embeben como configuración editable Alarm. El workspace mantiene el sidecar `_confirmed_tool_catalog_revision`, que NO es miembro de `AlarmConfiguration`. Source schema v2 está SUPERSEDED; no se agregó decoder legacy.

### Flujo vigente

```text
Save Draft  -> lee Confirmed Tool Catalog y fija Cn en workspace
Validate    -> valida configuración, Cn actual y todas las referencias Tool
Verify      -> concurrencia de Source gestionada por Manager
Publish     -> verifica otra vez Cn (drift guard) y congela ToolDependencyManifest(Cn)
```

El manifest incluye origins, todos los escalones de routing definidos —incluso inactivos— y visual targets de todas las Rules, incluidas inactivas. Un cambio Cn posterior no reinterpreta Rn/Cn; una Source histórica es inmutable. `VALID_AT_SAVE != READY != EFFECTIVE`.

## C1 — Tool ownership CURRENT

`web/tools/catalog` construye y persiste el Confirmed Tool Catalog CURRENT en Blob; `web/tools/discovery-cosmos` inspecciona conexiones Tool Cosmos nombradas y confirma revisiones; `web/tools/catalog-manager` implementa UI/callbacks de esa capability. El host temporal compone servicios. `backend/tools` SUPERSEDED. STRATEGIC puede ser destino de routing, pero no visual target mientras no exista su proyección visual acordada. El editor no usa latest Tool para corregir retrospectivamente un snapshot congelado.

## C2 — Source Key y topología

`domain/alarms/identity.py` declara la constante de **texto plano** `ALARM_CONFIGURATION_SOURCE_KEY = 'alarm-configuration'`. El host Web la convierte a `SourceKey` técnico para construir el módulo; Materialization, Runtime y Delivery la importan desde Domain, eliminando la variable ambiental homónima. La constante no suprime comprobaciones de `source_key` en proyecciones, manifest READY, selección exacta y EFFECTIVE.

El resource contract Cosmos EXISTENTE está en `web/alarms/projection-cosmos/storage.py`:

```text
logical_id       ada.command_center.alarms.configuration.projection
physical_name    ada-command-center-alarm-configuration-projection
partition_key    /partition_key
allowed_override CONNECTION_REF
```

Web lo resuelve al componer su conexión. Materialization consume el mismo contrato para nombre de contenedor y partición de validación; `ALARM_PROJECTION_CONTAINER` deja de ser variable de despliegue. La cuenta/base/credencial Cosmos de **entrada** para Materialization siguen siendo manuales y deben coincidir físicamente con las del host durable. No se validó aún esa coincidencia en infraestructura real.

**No confundir:** el nombre de contenedor **Blob** sí es ambiental y se mantiene. Las conexiones **de salida** de Delivery son independientes y se configuran mediante su registro nombrado. `APPLICATION=ada-command-center` identifica los tres procesos; `VOLUMEN_PATH` absoluta/compartida es una decisión operacional del desarrollador, no derivada de la constante de Source ni calculada automáticamente.

## Evidencia histórica y límites

B1d observó Confirmed Tool Catalog desde dos Tool Source/Projection controladas y una Alarm Source local. C1 cerró ownership Web estructural; C2 cerró la eliminación de divergencias contractuales de configuración. **UNVERIFIED:** Alarm Source Blob + Projection Cosmos durable E2E, Azure real, equivalencia física del montaje en jobs independientes y aceptación visual final del browser. Tener adapters en código no acredita ese gate.

## OPEN / frentes distintos

- C3: productor/verificadores de qualification reales, no deducidos a partir de `ALARM_QUALIFICATIONS_FILE`.
- C4: Delivery CURRENT-only; no cambia la semántica de Source/Tool authoring.
- C5: resolver pareja de evidencia técnica Runtime y auditoría del resto de `.env.detail`.
- UX: fin del turno, estado de guardado/modal y autonomía visual frente al routing siguen requiriendo decisión propia.

El cierre C2 no autorizó cambiar el agregado, Source schema v3, UI, routing, qualification ni los productores de datos físicos.
