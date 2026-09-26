# ADA Command Center — Current Implementation

Estado: **CURRENT / ALARM CONFIGURATION COMPONENT CHECKPOINT 2026-09-26 / OPERATIONAL INTEGRATION OPEN**

Corte auditado:

```text
moragaga/atlanticus@411aea44ac60c09d2b07ce41d34c3f378788b97b
moragaga/atlanticus-cannonical@83cd871c8418e37d2c29dff30e2ea5ef54bda4a0 (input)
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

## Paquetes CURRENT observados en main

```text
scopes/ada-command-center/
├── domain/
│   ├── alarms/                         # Incluye routing_policy.py
│   └── tools/
├── backend/
│   ├── alarms/
│   │   ├── core/
│   │   ├── materialization/             # Pure B.2; NO job de proceso
│   │   └── persistence/
│   ├── processes/
│   │   └── alarms-runtime/
│   └── tools/
│       └── catalog/
└── web/
    ├── alarms/
    │   ├── configuration/
    │   ├── persistence/
    │   ├── projection-local/
    │   └── projection-cosmos/
    └── application/
        └── ada-command-center-configuration-manager/
```

En este checkpoint **no** aparece `backend/processes/alarms-materialization`.

## Matriz de estado

| Elemento | Estado comprobable |
|---|---|
| Alarm Domain / Core | CURRENT / IMPLEMENTED |
| Command Center Tools Domain y Tool Catalog | CURRENT / IMPLEMENTED |
| Alarm Tool Reference reader / exact dependency manifest | CURRENT / IMPLEMENTED |
| Alarm Configuration Source schema v3 | CURRENT / IMPLEMENTED |
| Workspace pin + Validate/Publish Tool freeze | CURRENT / IMPLEMENTED |
| Alarm Source/Base Projection | CURRENT / IMPLEMENTED |
| Alarm Projection local/Cosmos stores y provider composition | CURRENT / IMPLEMENTED; E2E Azure UNVERIFIED |
| Strict routing policy + B.2 validator + Web guided selection | CURRENT / IMPLEMENTED; suites de componente VERIFIED |
| B.2 pure resolver + Runtime/Delivery candidate contracts | CURRENT / IMPLEMENTED |
| Host/browser completo posterior al routing | UNVERIFIED |
| Operational producer/consumer Blob/Cosmos en ambiente real | UNVERIFIED |
| B.2 Materialization process/job | PLANNED / NEXT |
| Artifact stores/descarga operacional para Runtime | OPEN |
| Runtime Effective Head/Adoption | PLANNED / AFTER |
| Alarm Live Delivery / Management Projection | PLANNED / SEPARATE |

## Contratos CURRENT

```text
AlarmConfiguration(rules, messages)
AlarmConfigurationSnapshot(configuration, tool_dependencies)
Source schema_version = 3
ToolDependencyManifest(Cn)
AlarmResolutionKey(Rn, Cn)
```

Schema v2 **SUPERSEDED** y sin decoder legacy. El aggregate Alarm no incorpora metadata workspace. El sidecar `_confirmed_tool_catalog_revision` fija Cn en Save Draft; Validate y Publish rechazan drift; publicación preserva evidencia histórica Tool exacta para B.2.

## Projection CURRENT

`AlarmConfigurationProjectionBuilder` consume Source y mantiene intacto el snapshot v3. `ProjectionRecord[AlarmConfigurationSnapshot]` dispone de codecs, stores local/Cosmos y composición de Source Local/Blob con Projection Local/Cosmos. El host temporal de pruebas usa providers locales. No confundir adapter de Cosmos existente con un job desplegado ni atribuir un productor operacional ya verificado.

## Strict routing CURRENT

```text
PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC -> END
```

No mismo nivel, retroceso ni saltos. Se permite cero destinos en C1/C2. C3 usa solo origen. C1 inmediato; C2 waits positivos acumulados en B.2 y deadlines absolutos respecto del inicio de ocurrencia en Core. Domain/B.2/Web comparten política. Strategic está en opciones de routing, no en visual targets.

## Evidencia de qualification del hito

El usuario ejecutó, después de los incrementos correspondientes, tests/lint/format de:

```text
domain/alarms                  GREEN (56 puntos visibles)
backend/alarms/materialization GREEN (49 puntos visibles)
web/alarms/configuration      GREEN (114 puntos visibles)
```

Es evidencia local proporcionada, no CI ni rerun demostrado del SHA final `411aea...`. Host/browser y Blob/Cosmos E2E siguen UNVERIFIED.

## Siguiente frontera

**Sólo** `backend/processes/alarms-materialization` como próximo foco, después de confirmar por código los contratos de adquisición de proyección exacta, qualification y salida. Debe producir Runtime/Delivery/findings de forma atómica usando el pure resolver existente, sin reutilizar latest Tool Catalog ni adelantar Adoption.

## Conflictos/no mezclar

- `Project 3.14.7` vs Command Center `==3.14.2` — OPEN / SEPARATE.
- `synchronize_visual_targets` actual deriva targets del routing, pero el canonical de UX afirma que ambos no deben condicionarse sin decisión. Consignar CONFLICT antes de rediseñar.
- No reintroducir adapters legacy ni crear esquemas, settings, contenedores o procesos nuevos distintos del foco autorizado.
