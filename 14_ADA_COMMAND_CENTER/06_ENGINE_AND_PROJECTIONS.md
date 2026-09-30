# ADA Command Center — Engine and Projections

Estado: **CURRENT — Source v3; READY local; WAL/EFFECTIVE; Runtime CURRENT v1 + FACTS v2; Delivery input exclusivamente último CURRENT; Alarm Configuration projection physical name `alarm-configuration` compartido entre local y Cosmos.** C1/C2/C4 CLOSED local; Resource Preparation/startup gate, Live, Management y Analytics siguen PLANNED. Código auditado `atlanticus@fe606cbefb932211b8329df9285004f4933df41d`.

## Source y materialización exacta

`AlarmConfigurationSnapshot(configuration, ToolDependencyManifest(Cn))` congela Rn/Cn. Materialization consume la proyección de entrada sin reinterpretar Tool latest; qualification manual externa y B.2 producen:

```text
Alarm Source Rn + Tool manifest Cn
  -> Alarm Projection Cosmos: alarm-configuration, PK /partition_key
  -> qualification externa (JSON manual CURRENT) + resolver B.2
  -> VOLUMEN_PATH/ada-command-center/alarms/materialization/
       ready.json
       versions/<result_id>/manifest.json
       versions/<result_id>/runtime.json
       versions/<result_id>/delivery.json
```

READY requiere pareja Runtime/Delivery íntegra, mismo pin y manifest/hash. BLOCKED deja diagnóstico sin sustituir READY. La salida monolítica previa B.2 a Cosmos está SUPERSEDED; no reintroducir adapters legacy.

## C2 preservado — identidad física y procesos

Los tres procesos usan `APPLICATION=ada-command-center` con `job_key`/leases individuales; `VOLUMEN_PATH` absoluta y manual debe apuntar al mismo volumen físico. Source Key única Domain: `alarm-configuration`.

El resource contract de Alarm Projection conserva:

```text
logical_id       ada.command_center.alarms.configuration.projection
physical_name    alarm-configuration
partition_key    /partition_key
```

El host local usa la misma identidad física bajo `AdaStorageNamespace`:

```text
<base_root>/conciencia_situacional/command-center/projections/alarm-configuration/
```

El adapter local no depende del paquete Cosmos para obtener esa identidad; ambos importan la constante compartida desde Alarm Configuration Web. Los bindings físicos cuenta/base Cosmos de Web/Materialization y los montajes reales siguen UNVERIFIED.

## Runtime y outputs CURRENT

Runtime selecciona `(source_key, result_id, manifest_sha256, resolution_key)` exacto, adopta en WAL y mantiene EFFECTIVE; READY por sí solo no es EFFECTIVE. Publica CURRENT v1 completo/reemplazable en `runtime/output/current/latest.json` y exporta FACTS v2 inmutables encadenados bajo `runtime/output/facts/`, con cursor productor `runtime/output/state/facts-export-cursor.json`.

Este hito no modificó Engine, WAL, CURRENT, FACTS ni adopción EFFECTIVE.

## Delivery input — CURRENT-only

`processes/alarms-delivery` consume sólo el último CURRENT. Valida estructura/SHA/source/timestamps y exige igualdad de pin exacto con EFFECTIVE y READY. Persiste `delivery/input/current/latest.json` bajo fence cuando corresponde. No consume FACTS como backlog y no produce Live.

Se preservan los estados y reglas C4 vigentes (`WAITING_EFFECTIVE`, `WAITING_CURRENT`, `EFFECTIVE_CHANGED`, `STALE_SOURCE`, `CURRENT_UNCHANGED`, `CURRENT_STAGED`) y la semántica de cero alarmas mediante `alarms=[]`.

El bootstrap conserva `config/connections.json`, `ParallelCosmosPublisher` y `ALARM_DELIVERY_MAX_WORKERS`; no fueron refactorizados aquí.

## Fronteras de proyección

- **Alarm Configuration local projection:** CURRENT en filesystem, con root físico `.../projections/alarm-configuration/`.
- **Alarm Configuration Cosmos projection:** CURRENT por contrato, container `alarm-configuration`, PK `/partition_key`.
- **Engine CURRENT:** estado operacional actual con prioridad resuelta.
- **Delivery input:** recepción/validación CURRENT y almacenamiento local; no es Live.
- **AlarmLiveProjection:** NOT IMPLEMENTED.
- **Management Capture/Projection:** PLANNED separado.
- **History/Analytics:** PLANNED separado; Web no lee WAL ni deriva History del inbox CURRENT.

## Resource Preparation — frontera siguiente, no implementación actual

El Project acordó como dirección que los recursos no deben duplicar convenciones entre local y durable. El próximo incremento debe revisar cómo reutilizar namespaces/resource contracts y cómo ejecutar un preparation worker/startup gate antes de habilitar Web o procesos dependientes.

Estado de esa frontera:

```text
Resource Preparation + startup gate   PLANNED
Tool Catalog filesystem local          NOT IMPLEMENTED
ensure local roots                     UNVERIFIED / por diseñar contra código existente
ensure Blob/Cosmos resources           existe infraestructura parcial en ADA Generic; integración CC UNVERIFIED
startup dependency/gate                UNVERIFIED / por revisar
```

No declarar aún que `local` completo es filesystem ni que `durable` completo está provisionado automáticamente.

## Evidencia de este hito

**VERIFIED local:** 123 PASS Alarm Configuration Web + 5 PASS Projection Cosmos + 28 PASS Configuration Manager = **156 PASS**, además de `git diff --check` limpio.

**UNVERIFIED:** CI, Docker separado, Azure, mismo Cosmos físico Web/Materialization y startup gate real.

**PLANNED / único siguiente foco:** Resource Preparation + startup gate. No abrir Live, Management, History, nueva UX ni dashboard en ese incremento.
