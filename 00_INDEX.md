# Atlanticus Canonical Context — Index

Estado: **CURRENT — ALARM BACKEND LIVE BASELINE CLOSED / ADA WEB STATIC ALARM BASELINE CLOSED**

## Autoridad de este cierre

```text
Implementation        moragaga/atlanticus@686a80f6a05eeea93d35d642cf2f92100cb1e61b
Decisions             moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical pre-replace moragaga/atlanticus-cannonical@44d3c803f60d1a1630d3a3374a663447cfe21248
Git                   SOLO LECTURA
```

## Estado por frente

| Ubicación | Estado relevante |
|---|---|
| `01_CURRENT_STATE.md` | Alarm backend materialization → runtime → modeler → delivery → Cosmos sigue CURRENT/CLOSED. |
| `03_DECISIONS_CURRENT.md` | Contratos Tool Structure / Render Topology / Alarm Baseline añadidos como CURRENT/FROZEN. |
| `04_ALARM_ENGINE/` | No modificado por este hito. Dynamic alarm lifecycle y live projections siguen ownership del backend. |
| `10_MANAGER/04_TOOL_CONFIGURATION.md` | Layout roles SUPERSEDED; `center_component_key` + `ToolRenderTopology.bottom_component_key` CURRENT. |
| `11_ADA_GENERIC/` | Tool READY deriva render binding + static alarm baseline; UNCONFIGURED sigue arrancando sin Tool implícita. |
| `13_ADA_WEB/` | Static Alarm Baseline Projection/Surface CURRENT; visual browser qualification OPEN. |
| `14_ADA_COMMAND_CENTER/` | No reabierto en este cierre. |
| `16_KPI_BACKEND_RECOVERY/` | No reabierto en este cierre. |
| `17_DISTRIBUTION_AND_TOOLING/` | Frente separado; no reabierto en este cierre. |

## Checkpoint de este hito

```text
TOOL-RENDER-TOPOLOGY                      CLOSED / VERIFIED / CURRENT
ADA-WEB-STATIC-ALARM-BASELINE             CLOSED / VERIFIED / CURRENT
ADA-GENERIC-STATIC-BASELINE-INTEGRATION   CLOSED / VERIFIED / CURRENT
VISUAL-BROWSER-QUALIFICATION              OPEN / NEXT FOR THIS TRACK
DYNAMIC-ALARM-OVERLAY                     PLANNED / SEPARATE
```

## Frontera

El baseline implementado es estático:

```text
ToolConfiguration
    + ToolStructure
    + ToolRenderTopology
        ↓
OperationalRenderBinding
        ↓
AlarmBaselineProjection
        ↓
AlarmBaselineSurface
        ↓
ADA Generic
```

No contiene:

```text
alarm runtime state
routes
origin/affected
cards
preview
selection
severity coloring
management
history
analytics
```

## Referencia visual

`moragaga/isolated-web-functions:main` / `operational_trace` es REFERENCE para geometría e interacción futura.

No es autoridad arquitectónica ni contractual.

## Siguiente frontera de este track

```text
ADA-WEB-STATIC-BASELINE-VISUAL-QUALIFICATION
```

Objetivo: levantar ADA Generic y validar visualmente Process sin bottom, Process con bottom e Integrated Operations, sin abrir todavía overlay dinámico de alarmas.
