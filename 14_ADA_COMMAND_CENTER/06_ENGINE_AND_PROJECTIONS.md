# ADA Command Center — Engine and Projections

Estado: **CURRENT SOURCE V3, PROJECTION ADAPTERS, PURE B.2 AND PROCESS / LOCAL ARTIFACT PUBLICATION PLANNED**

## Base Alarm Configuration y evidencia exacta

`AlarmConfigurationSnapshot(configuration, ToolDependencyManifest(Cn))` de Source release Rn. La base/operational projection conserva ese snapshot sin leer latest Tool Catalog. El codec y stores Local/Cosmos existen; operación real sobre Blob/Cosmos permanece UNVERIFIED.

## Materialization como única frontera de adquisición para B.2

```text
Alarm Source Rn / ToolDependencyManifest(Cn)
 -> operational ProjectionRecord[AlarmConfigurationSnapshot] en Cosmos
 -> Alarm Materialization process
 -> qualifications de procedencia comprobada + pure B.2
 -> READY/BLOCKED
```

Materialization 0.2.1 está implementado, pero **todavía publica Runtime, Delivery, manifest y findings en un documento Cosmos**. Ese store de salida está **SUPERSEDED como diseño**.

## Contrato siguiente decidido: volumen local

```text
READY:
  RuntimeAlarmConfiguration [AlarmResolutionKey(Rn,Cn)]
  DeliveryAlarmConfiguration [misma AlarmResolutionKey(Rn,Cn)]
  manifest con procedencia/integridad
  todo publicado como unidad observable en VOLUMEN_PATH

BLOCKED:
  findings recuperables;
  sin Runtime/Delivery ejecutables ni promoción de candidato inválido.
```

Runtime/Engine y Delivery recuperarán **localmente** sus contratos al inicio de su ciclo de vida y conservarán el artefacto exacto adoptado. No deben descargar esas configuraciones desde Cosmos. No congelar aún layout/rutas/codec de archivos: revisar primero los componentes locales existentes. Materialization publica READY; **no controla EFFECTIVE**.

## Runtime, Delivery, Live, Management — fronteras separadas

`READY != EFFECTIVE`: Runtime Adoption es posterior y administra el paso de una key completa al estado efectivo. El Engine ya contiene sesiones y helpers de Adoption, no un flujo completo integrado desde archivos locales. Delivery sólo puede combinar la configuración correspondiente al mismo exact Effective key con el estado operacional ya resuelto por Engine. Las proyecciones Live y Management tienen otro owner: Live es current operational state; Management es historial de acciones. No usar el cambio de almacenamiento de configuración como decisión prematura sobre la posterior publicación Live a Web.

No consultar Tool Cosmos individual ni leer WAL como API de Live Delivery. El Confirmed Tool Catalog durable objetivo termina en Storage; las referencias Tool congeladas viajan con el snapshot Alarm.

## Orden de trabajo

1. Cerrar salida local de Materialization y tests unitarios/de integración local (**único foco siguiente**).
2. E2E Cosmos/Blob sólo cuando exista infraestructura, sin bloquear gates locales.
3. Otro incremento: lector local exacto y Runtime Adoption/Effective Head.
4. Otro incremento: Delivery local + Live; después Management Capture/Projection.
