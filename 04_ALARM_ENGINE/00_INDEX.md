# Alarm Engine — Index

Estado: **CURRENT / MATERIALIZATION LOCAL + RUNTIME B1 IMPLEMENTADOS / EFFECTIVE GLOBAL PLANNED**

Corte: `atlanticus@c8f23d91ae1cb817be55b4b812b22ffca518880e`; canonical base revisada `58241ddb6db5adbd2e783c7ec9f456f1bda5a321`; decisions `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`. Actualización documental propuesta, no modificación remota.

| Archivo | Responsabilidad | Estado en este corte |
|---|---|---|
| `01_DOMAIN_MODEL.md` | Dominio, identidad y contratos Core. | CURRENT; sin sustitución en este cierre. |
| `02_RUNTIME_AND_LIFECYCLE.md` | Lifecycle, prioridad y routing. | CURRENT; sin sustitución. |
| `03_PERSISTENCE_AND_RECOVERY.md` | WAL, durable/materialized y recovery. | CURRENT para Engine; adopción global todavía PLANNED. |
| `04_CONCURRENCY_LEASES_AND_FENCING.md` | Authority, fencing y takeover. | CURRENT; deben respetarse en futura adopción. |
| `05_PROJECTION_AND_PUBLICATION.md` | Source/proyección, publicación local B.2 y futuros consumidores. | ACTUALIZAR: salida local CURRENT, no PLANNED. |
| `06_MANAGEMENT.md` | Management y deactivation. | CURRENT; sin sustitución. |
| `07_CONFIGURATION_AND_MATERIALIZATION.md` | Source v3, B.2, job local, lector e identidad exacta. | ACTUALIZAR: Incrementos A/B1. |
| `08_QUALIFICATION_BASELINE.md` | Campaña R3.5 y evidencia local reciente delimitada. | ACTUALIZAR evidencia, preservar genealogía. |
| `09_DECISION_INDEX.md` | Decisiones actuales, refinamientos y conflictos. | ACTUALIZAR A/B1. |
| `10_OPEN_ITEMS.md` | OPEN verificables y siguiente frontera. | ACTUALIZAR hacia Runtime Adoption durable. |
| `11_SOURCE_LEDGER.md` | Checkpoints, rutas, tests, límites. | ACTUALIZAR a SHAs y resultados de este corte. |
| `12_COMMAND_CENTER_ANALYTICS_BOUNDARY.md` | Engine vs Analytics. | SEPARATE, no tocar. |
| `13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md` | Identidad y planificación B1 vs adopción y EFFECTIVE pendientes. | ACTUALIZAR distinción CURRENT/PLANNED. |

## Cadena actual frente a etapas posteriores

```text
Alarm Source Rn + ToolDependencyManifest(Cn)
  -> ProjectionRecord[AlarmConfigurationSnapshot] (adaptadores Local/Cosmos)
  -> [CURRENT] Materialization adquiere proyección Cosmos y qualifications
  -> [CURRENT] resolver B.2 puro
  -> [CURRENT] publicación local READY: manifest + Runtime + Delivery, misma Rn/Cn
     o BLOCKED: manifest/findings, sin pareja ejecutable ni promoción READY
  -> [CURRENT] RuntimeLocalConfigurationReader: READY actual o artefacto exacto
  -> [CURRENT] AlarmConfigurationArtifactRef + revisión y plan B1
  -> [PLANNED] ejecución/adopción global durable, recovery y EFFECTIVE
  -> [PLANNED / SEPARATE] Delivery local exacto y Live
```

`READY != EFFECTIVE`. El planificador B1 no autoriza a presentar su resultado como adopción durable. No releer Cosmos desde Runtime para cargar los contratos B.2. Próximo foco técnico: **diseño de Runtime Adoption durable**, no otra vuelta a Materialization ni una integración simultánea de Delivery.
