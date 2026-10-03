# Alarm Engine — Configuration and Materialization

Estado: **CURRENT implementation + DECIDED target boundary; semantic simplification PLANNED**.

Checkpoint:

```text
atlanticus@6725237a19c4442fdfa1b32c3410c124e9348dbc
```

## Published configuration CURRENT

La configuración compartida vive en `ada-contracts-alarms`.

```text
AlarmConfigurationSnapshot
    configuration: AlarmConfiguration
    tool_dependencies: ToolDependencyManifest
```

`ToolDependencyManifest` vive en `ada-contracts-tools`.

El snapshot conserva la revisión exacta del Tool Catalog usada para publicar y es la frontera durable entre authoring y consumidores downstream.

## Ownership semántico DECIDED

Antes de publicar, Command Center debe completar:

```text
authoring
business validation
Tool reference validation/resolution
routing validation
visual target validation
publication
```

No publicar configuración inválida.

Por contrato, snapshot publicado significa **ready/materializable**, no "candidato que Materialization debe volver a calificar".

## Materialization target PLANNED

```text
AlarmConfigurationSnapshot publicado
        ↓
transformación determinista
        ├── RuntimeAlarmConfiguration
        └── DeliveryAlarmConfiguration
```

Materialization no debe consultar Tools ni repetir semantic resolution ya cerrada upstream.

`ResolvedAlarmConfiguration` como stage publicado separado está **SUPERSEDED**.

## Implementación CURRENT

El cutover `ada-contracts` cambió ownership/imports y preservó wire/document contracts. No reescribió todavía la lógica existente de Materialization.

Permanece deuda técnica en:

- dependencias hacia Web/projection contracts;
- semantic resolution downstream;
- qualification previa que debe moverse definitivamente upstream.

Esta deuda no debe esconderse con compatibility shims.

## Pin / adoption invariants — CURRENT

Se conservan:

```text
READY != EFFECTIVE
exact artifact pin
source_key + result_id + manifest_sha256 + resolution_key
Runtime y Delivery deben usar el mismo artefacto exacto
no fallback a latest READY
```

El job fijado no reinterpreta latest en cada ciclo.

## Engine publication schemas — CURRENT

Autoridad física:

```text
ada-contracts-alarms
└── ada/contracts/alarms/schemas/
```

Las copias históricas bajo `backend/alarms/contracts` ya no existen en `main@6725237...`.

El gate debe verificar que builds/distribuciones carguen los schemas desde el package autoritativo; no reintroducir duplicados sólo para conservar paths históricos.

## Frontera siguiente

No simplificar Materialization durante el siguiente chat.

Primero:

```text
Command Center capability parity
→ retomar qualifier ada-contracts
→ cerrar gate
```

La limpieza semántica de Materialization y extracción física del Engine son incrementos separados.
