# ADA Command Center — Domain Ownership and Migration

Estado: **CURRENT — C1 Tool owners Web CLOSED; C2 Source Key Domain compartida CLOSED; C4 recepción Delivery CURRENT-only CLOSED localmente sin mover owners. Proyección Cosmos y distribución física pendientes independientes.** Checkpoint `atlanticus@45eff96d777f4711cb011f779ffc0a6c87bf0ca4`.

## Domain Alarms — CURRENT / C2 preservado

```text
scopes/ada-command-center/domain/alarms
ada-command-center-alarms-domain==1.0.0
```

Domain posee `AlarmConfiguration`, `AlarmConfigurationSnapshot` v3 e identidad transversal **de texto plano** `ALARM_CONFIGURATION_SOURCE_KEY = 'alarm-configuration'` en `identity.py` exportada desde `__init__.py`. No conoce `SourceKey` Web, Cosmos, Blob ni rutas de jobs. El host Web construye `SourceKey(ALARM_CONFIGURATION_SOURCE_KEY)` al componer servicios; Materialization/Runtime/Delivery importan el literal y lo exponen en settings. La identidad se sigue verificando sobre proyecciones, READY/EFFECTIVE y snapshots actuales.

El carácter transversal de Source Key justifica este contrato Domain C2, pero **no** justifica trasladar infraestructura a Domain.

## Domain Tools — CURRENT previo a C2

```text
scopes/ada-command-center/domain/tools
ada-command-center-tools-domain==1.0.0
ToolDependencyEntry
ToolDependencyManifest
```

Los tipos transversales de manifest se separan de consolidación física. La dirección tras C1 se mantiene: contratos `ada-web-tools` → Domain Tools → wrapper Snapshot Domain Alarms. `domain/alarms` tiene dependencias, no es módulo vacío. La normalización pendiente de ciertos tipos estructurales en `ada-web-tools` NO fue abordada por C2/C4.

## Tool services Web — C1 CLOSED

```text
web/tools/catalog           -> snapshots, consolidación y Blob CURRENT
web/tools/discovery-cosmos  -> conexiones nombradas, inspect/confirm/adopted
web/tools/catalog-manager   -> UI y callbacks capability-local
```

`backend/tools` y namespaces anteriores son SUPERSEDED. Host temporal compone servicios sin duplicarlos. Python server-side usado exclusivamente por Web pertenece a Web; Domain no absorbe adapters por cercanía.

## Backend Alarm y frontera abierta

```text
backend/alarms/materialization          -> resolver B.2, codec y lector READY exacto
backend/alarms/core                     -> Engine puro
backend/alarms/persistence              -> WAL/EFFECTIVE/fencing
backend/processes/alarms-materialization
backend/processes/alarms-runtime
backend/processes/alarms-delivery
```

C2 unificó `APPLICATION=ada-command-center` manteniendo `job_key`/leases propios. `VOLUMEN_PATH` sigue manual/absoluto y apunta a raíz `VOLUMEN_PATH/ada-command-center/alarms`. El usuario confirmó que no hay despliegue legacy que migrar.

**C4 delta:** `backend/processes/alarms-delivery` sigue siendo owner del receptor; sus 14 archivos cambiados en Git lo dejan consumiendo último CURRENT exclusivamente y validando pin/EFFECTIVE/READY exactos. FACTS v2 y cursor productor permanecen en Runtime. No se creó otro proceso, servicio Live o adapter, ni se movió infraestructura.

**Dependencia OPEN:** Materialization importa actualmente adaptador y resource contract de `web/alarms/projection-cosmos`. C2 solo retiró nombre Cosmos ambiental duplicado, tomando `ALARM_CONFIGURATION_PROJECTION_STORAGE_RESOURCE` físico; C4 no tocó el límite. Futura decisión de ownership requiere responsabilidad técnica real, sin mover todo a Domain ni crear shims.

## Web Alarm Source/Projection CURRENT

`web/alarms/configuration` gestiona authoring/Source/Release; adapters existentes residen en `web/alarms/persistence`, `web/alarms/projection-local`, `web/alarms/projection-cosmos`; `web/application/ada-command-center-configuration-manager` compone cliente, principal y bindings. El nombre `ada-command-center-alarm-configuration-projection` y partición `/partition_key` vienen del resource contract Web actual. Igual contrato físico **no prueba** misma cuenta/base: Materialization conserva endpoint/base/credencial por despliegue. Blob container sigue configurable. Delivery tiene registry Cosmos de salida independiente: no derivar su conexión del input Alarm Projection.

## Fronteras siguientes sin ampliación C4

- **C3 PLANNED/BLOCKED:** qualification GREEN real requiere productor/verificadores identificados; archivo manual es CURRENT legítimo.
- **C4 CLOSED local:** recepción de último CURRENT únicamente; Runtime FACTS/WAL permanecen CURRENT. Qualification física independiente pendiente.
- **C5 PLANNED:** contrato de technical evidence y env restante según dueño real.
- **Próximo foco propuesto:** qualification de Runtime/Delivery como procesos Docker independientes y misma raíz física, no refactor de dominios.
- **SEPARATE:** Starter Command Center, Live materializer, Management, History/Analytics, Azure/CI y validación visual.

## No legacy / no inferencia

No restaurar `backend/tools`, convertir Domain en infraestructura, inventar migraciones o alias de Source, añadir nombres físicos Cosmos a env ni inferir rutas/montajes del desarrollador. No suprimir FACTS del productor Runtime ni atribuir implementación Live al receptor C4. Contratos antes que consumidores; debate antes de nuevos cambios.
