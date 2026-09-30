# ADA Command Center — Configuration Scope

Estado: **CURRENT — Alarm Configuration Snapshot Source v3 / Tool dependencies Rn/Cn; C1 Web ownership, C2 identidad compartida y naming físico de Alarm Projection implementados. C4 Delivery CURRENT-only CLOSED en otro owner. Durable E2E y Resource Preparation/startup gate permanecen UNVERIFIED/PLANNED.** Checkpoint `atlanticus@fe606cbefb932211b8329df9285004f4933df41d`.

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

Definiciones Tool no se embeben en configuración Alarm editable. El workspace conserva sidecar `_confirmed_tool_catalog_revision`, no miembro de `AlarmConfiguration`. Source schema v2 está SUPERSEDED sin decoder legacy.

### Flujo vigente

```text
Save Draft -> consulta Confirmed Tool Catalog y fija Cn en workspace
Validate   -> revisa configuración/Cn actual y referencias Tool
Verify     -> concurrencia de Source bajo Manager
Publish    -> vuelve a comprobar Cn (drift guard) y congela manifest Cn
```

El manifest incluye origins, escalones de routing definidos y visual targets según contrato actual. Cambios Cn posteriores no reinterpretan snapshots Rn/Cn inmutables. `VALID_AT_SAVE != READY != EFFECTIVE`.

## C1 — Tool ownership CURRENT

`web/tools/catalog` construye/persiste Confirmed Tool Catalog CURRENT en Blob; `web/tools/discovery-cosmos` inspecciona conexiones Tool Cosmos nombradas y confirma revisiones; `web/tools/catalog-manager` posee UI/callbacks. Host temporal compone services; `backend/tools` está SUPERSEDED. El editor no reinterpreta snapshots congelados con Tool latest.

**Límite actual:** `ADA_MANAGER_PERSISTENCE_PROVIDER=local` no convierte todavía Tool Catalog a filesystem. El host temporal sigue requiriendo Storage para Tool Catalog en ambos providers. Un adapter local de catálogo forma parte del próximo frente de Resource Preparation/persistencia local, no de este hito.

## C2 — Source Key y topología CURRENT

`domain/alarms/identity.py` define `ALARM_CONFIGURATION_SOURCE_KEY = 'alarm-configuration'`; Web lo transforma en `SourceKey` técnico y Materialization/Runtime/Delivery lo consumen. La constante no elimina las verificaciones de `source_key` persistido.

La identidad física compartida de Alarm Projection reside en:

```text
web/alarms/configuration/resources.py
ALARM_CONFIGURATION_PROJECTION_PHYSICAL_NAME = 'alarm-configuration'
```

El resource contract Cosmos en `web/alarms/projection-cosmos/storage.py` queda:

```text
logical_id        ada.command_center.alarms.configuration.projection
physical_name     alarm-configuration
partition_key     /partition_key
allowed_override  CONNECTION_REF
```

El nombre anterior:

```text
ada-command-center-alarm-configuration-projection
```

está **SUPERSEDED**. No mantener alias ni compatibilidad legacy: no existe despliegue previo que migrar para este cambio.

Web durable resuelve conexión Cosmos por fuera del resource contract. Materialization reutiliza nombre/partición y no duplica `ALARM_PROJECTION_CONTAINER`.

## Namespace Storage/local CURRENT

El namespace lógico de Command Center se mantiene:

```text
application_namespace = conciencia_situacional
tool_namespace        = command-center
```

Storage/Source utiliza ese namespace:

```text
conciencia_situacional/command-center/tool-catalog/current.json
conciencia_situacional/command-center/sources/alarm-configuration/...
```

La proyección local ahora conserva la identidad física del recurso durable:

```text
<base_root>/conciencia_situacional/command-center/projections/alarm-configuration/...
```

El container Cosmos durable correspondiente es:

```text
alarm-configuration
PK /partition_key
```

Esto alinea la identidad física de Alarm Projection entre filesystem y Cosmos sin acoplar el adapter local al paquete Cosmos. No generalizar este hecho a Tool Catalog u otros recursos todavía no implementados.

## Configuración física todavía OPEN

Cuenta/base/credencial Cosmos de entrada Materialization siguen siendo configuradas externamente y deben coincidir físicamente con el host durable: UNVERIFIED. El contenedor Blob permanece ambiental. Las conexiones Tool Cosmos siguen siendo múltiples/nombradas cuando corresponde.

`APPLICATION=ada-command-center` continúa común entre los tres jobs. `VOLUMEN_PATH` absoluta/compartida la define el operador y no se deriva de Source.

## Evidencia de este hito

Commit:

```text
atlanticus@fe606cbefb932211b8329df9285004f4933df41d
```

Regresión local aislada reportada:

```text
Alarm Configuration Web                         123 PASS
Alarm Projection Cosmos                           5 PASS
ADA Command Center Configuration Manager         28 PASS
TOTAL                                            156 PASS
git diff --check                                 PASS
```

No certifica Source Blob + Cosmos E2E, Docker, Azure ni equivalencia física multi-host.

## OPEN y frentes distintos

- **Resource Preparation + startup gate:** PLANNED / NEXT; debe reutilizar contratos existentes y asegurar recursos antes de iniciar consumidores. Diseño aún no implementado.
- **Tool Catalog local:** NOT IMPLEMENTED; no declarar `local` totalmente filesystem hasta cerrar ese adapter.
- **C3:** productor/verificadores GREEN reales; `ALARM_QUALIFICATIONS_FILE` manual continúa CURRENT.
- **C5:** contract key/version de technical evidence y auditoría `.env.detail`, sin inventar valores.
- **Docker/distribución:** UNVERIFIED.
- **UX/END_OF_SHIFT operacional:** frente separado.

No utilizar este cierre documental para crear containers, variables, servicios o adapters no existentes.
