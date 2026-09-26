# ADA Command Center — Open Items

Estado: **OPEN / MANAGER UX PRIORITY / COSMOS ADAPTER IMPLEMENTED, AZURE E2E UNVERIFIED**

Checkpoint:

```text
moragaga/atlanticus@880cb692054c2cd78cdc29cf62ab7b16bbd2c3d6
```

## CLOSED

- Alarm Domain extraction.
- Tool Catalog V1.
- structured Alarm Configuration authoring (base contractual; UX definitiva IN PROGRESS).
- Pure B.2 resolver.
- Command Center `domain/tools`.
- ToolDependencyManifest.
- historical Tool name/structure capture.
- AlarmConfigurationSnapshot v3.
- Source schema v3.
- workspace Tool revision pin.
- validate/publish Tool drift protection.
- exact Alarm release -> Tool revision correlation.

## CURRENT PRIORITY — single focus

```text
ALARM-CONFIGURATION-MANAGER-UX
```

Cerrar la autoría amigable por familias, creación guiada de Rule/Message, `alarm_key` estable,
validaciones útiles, ayudas, restricciones C1/C2/C3 y elección de Tool/Component/Subcomponent.
Completar validación funcional en navegador, persistencia y el flujo Source/Projection del Manager.
El alcance y la información de Delivery diferida están en
`18_ALARM_AUTHORING_UX_AND_VISUAL_PRESENTATION.md`.

## Cosmos — estado revisado, despliegue abierto

En `atlanticus:main@7b61eaea463bab10a595166fa12d015e4c015c78` están implementados
`CosmosAlarmConfigurationProjectionStore` y la composición con Source Blob/local y
Projection Cosmos/local. Su presencia reemplaza la afirmación anterior de que el adapter
estaba por crear. Pruebas reales contra Azure, configuración/deployment y reconciliación del
resto de canonical: UNVERIFIED / OPEN, en otro foco.

No mezclar el Manager UX con Materialization Process o Live Delivery.

## AFTER

### B.2 Materialization Process

Consumes operational Alarm Projection and embedded Tool manifest.

### Tool qualification producer

OPEN.

### Evaluator qualification producer

OPEN.

### Runtime/Delivery/findings stores

OPEN.

### Runtime Adoption / Effective Head

PLANNED.

### Live Delivery

PLANNED.

### Management Capture / Management Projection

PLANNED / separate.

### UI notification of newer Tool revision

Explicit UX for saved C1 vs current C2 remains OPEN.

### Domain/catalog normalization

DEFERRED.

### Source v2 existing data

UNVERIFIED in target deployments.
No legacy reader exists.

### Python metadata

```text
3.14.7 target
3.14.2 current Command Center metadata
```

OPEN / SEPARATE.
