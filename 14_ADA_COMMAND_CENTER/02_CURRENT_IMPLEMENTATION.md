# ADA Command Center — Current Implementation

Estado: **CURRENT COMPONENTS / MATERIALIZATION PROCESS v0.2.1 PRESENT / LOCAL OUTPUT CHANGE OPEN**

Corte: `atlanticus@b600ca591b56d0924aed752dfae6e9fab2c6f1d6`; canonical inspeccionado `772d15078c97802d58d8b658b0d5d5b928fa2ed5`; decisions `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`. La auditoría de este cierre se limita a Materialization y sus contratos directos, no revalida todos los módulos del Command Center.

## Componentes CURRENT relevantes

```text
scopes/ada-command-center/
  domain/alarms/                  # Alarm Configuration v3 + routing policy
  domain/tools/                   # ToolDependencyManifest congelado
  backend/alarms/core/            # Engine contracts
  backend/alarms/materialization/ # Pure B.2 resolver
  backend/alarms/persistence/     # WAL/snapshots del Engine
  backend/processes/alarms-runtime/
  backend/processes/alarms-materialization/  # nuevo proceso v0.2.1
  web/alarms/configuration/
  web/alarms/projection-local/
  web/alarms/projection-cosmos/
  web/alarms/persistence/
```

## Estado comprobable por código

| Elemento | Estado |
|---|---|
| Domain, Core, Source schema v3 y ToolDependencyManifest | CURRENT / IMPLEMENTED. |
| Workspace pin Cn + Validate/Publish drift guard | CURRENT / IMPLEMENTED. |
| Alarm Configuration codec/builder y stores Local/Cosmos | CURRENT / IMPLEMENTED; Azure real UNVERIFIED. |
| Strict routing Domain/B.2/Web | CURRENT / IMPLEMENTED. |
| Pure B.2, Runtime/Delivery contracts, READY/BLOCKED | CURRENT / IMPLEMENTED. |
| Proceso ejecutable Materialization 0.2.1 con `__main__`, bootstrap y `execute_job` | CURRENT / IMPLEMENTED; tests integrados 0.2.1 UNVERIFIED. |
| Lectura de proyección Cosmos y JSON qualifications | CURRENT EN CÓDIGO; despliegue real UNVERIFIED. |
| Publicación de resultado único en Cosmos | CURRENT EN CÓDIGO / SUPERSEDED EN DISEÑO. |
| Artefactos versionados e íntegros en volumen para Engine/Delivery | DECIDED / PLANNED, aún no en código. |
| Runtime session/adoption helpers | CURRENT / PARTIAL; no consumo local completo ni Effective Head global confirmado. |
| Live Delivery y Management Projection | PLANNED / SEPARATE. |

## Precisión sobre materialization y tests

El código `backend/processes/alarms-materialization/publication.py` implementa hoy `CosmosAlarmMaterializationResultStore`; `composition.py` construye esa publicación. Debe ser reemplazada, no mantenida como alternativa legacy. La read-side Cosmos del input **sí permanece**. El proceso usa proveedor manual JSON de qualification; no significa integración del productor GREEN o Evaluator.

Evidencia local v0.2.0 del usuario: sincronización de dependencias correcta, wheel generado, 11 fallos por fixture Tool `tool-a` inválida, cinco findings de imports y diez pendientes de formatter. El HEAD actual 0.2.1 contiene correcciones (`tool_a`), pero no existe evidencia aportada de rerun completo. No hay Cosmos disponible para E2E. La anterior qualification de pure B.2 no acredita automáticamente este job.

## Contratos que el cambio no debe alterar

```text
AlarmConfigurationSnapshot v3 (configuration, tool_dependencies)
ToolDependencyManifest exacto de Rn/Cn
AlarmResolutionKey(alarm_configuration_revision, confirmed_tool_catalog_revision)
READY -> Runtime y Delivery con key idéntica
BLOCKED -> findings, sin artefactos ejecutables
READY != EFFECTIVE
```

**Después de cerrar la salida local:** otro frente conectará Runtime Adoption y la lectura local exacta; Delivery continuará después. El Editor Web conserva su propio foco y no se modifica en este incremento.
