# Alarm Engine — Decision Index

Estado: **CURRENT (2026-10-10) — decisiones congeladas conservadas; implementación revisada en `atlanticus@c3b8ed3b8de4bbafdaeeff4410d4daaa20bed1b4`; diferencias generacionales documentadas**.

| Frontera | Estado |
|---|---|
| Business Alarm model | CURRENT / frozen. |
| Source snapshot + exact Tool manifest | CURRENT. |
| Shared Tool contracts | CURRENT in `ada-contracts-tools`. |
| Shared Alarm contracts | CURRENT in `ada-contracts-alarms`. |
| Published valid configuration | CURRENT: `AlarmConfigurationSnapshot`. |
| Public `ResolvedAlarmConfiguration` stage | SUPERSEDED. |
| Materialization Runtime+Delivery pair | CURRENT implemented / SUPERSEDED target. |
| Materialization Runtime+Modeler+Delivery split | CURRENT decision / PLANNED implementation. |
| READY/EFFECTIVE exact pin | CURRENT / frozen. |
| Nuevo Runtime CURRENT durable v1 / FACTS stream v4 | CURRENT implementado; consumo por Modeler histórico UNVERIFIED. |
| Direct Delivery input CURRENT+FACTS | CURRENT implemented / SUPERSEDED target. |
| Runtime→Modeler durable ordered handoff semantics | CURRENT decision / physical schema OPEN. |
| Modeler stateful backend responsibility | CURRENT decision / PLANNED implementation. |
| Modeler→Delivery latest-wins per destination | CURRENT decision / physical schema OPEN. |
| CAROUSEL six-slot semantics | DESIGN FROZEN partial. |
| QUEUE_IN_QUEUE Mine/Plant topology | DESIGN FROZEN partial. |
| QUEUE_IN_QUEUE fairness | OPEN. |
| Rotation window 90/120 s | OPEN. |
| Modeler config adoption with live state | OPEN. |
| Extracción del nuevo `ada-alarm-engine` | CURRENT para Runtime; integración downstream productiva OPEN. |
| History / Analytics | PLANNED / separate. |

## Refinamiento de estado (2026-10-10)

- **CURRENT por código:** el Runtime actual confirma WAL y publica `ada_alarm_engine_durable_current_state` v1 y facts stream/cursor v4. El contrato histórico CURRENT v1 (`ada_command_center_engine_resolved_current_state`) y FACTS v2/v3 no se consideran equivalentes.
- **CURRENT:** checkpoints WAL, compactación con fencing y política configurable de iteración (`iteration_summary_every=1` por defecto).
- **VERIFIED local:** stress sintético v3 readjudicado 7/7 y 553 pruebas acotadas.
- **OPEN / no decisión nueva:** interfaz exacta que conectará el nuevo CURRENT/FACTS con Modeler/Delivery; footprint/retención de facts; evaluadores productivos.
- **Decisions:** no se alteran las fronteras congeladas Live / Management / History-Analytics ni las decisiones existentes de scheduler de Modeler.

## Refinements from this closure (historical decision record)

1. El Engine target incorpora un `Modeler` backend lógico stateful entre Runtime y Delivery.
2. El actual `DeliveryAlarmConfiguration` se reconoce como mezcla de Modeler + Delivery concerns.
3. Materialization target pasa de dos artefactos a tres: Runtime / Modeler / Delivery.
4. Runtime conserva la verdad operacional; Modeler conserva estado lógico de slots/colas/timers; Delivery queda transporte/publicación.
5. El consumo directo Runtime → Delivery sigue siendo CURRENT implementado pero queda SUPERSEDED como target.
6. Runtime nunca espera a Modeler y Modeler nunca espera a Delivery para persistir su propio estado.
7. Runtime → Modeler requiere stream/handoff durable, ordenado y no-drop con checkpoint del consumidor.
8. Modeler → Delivery usa latest-wins por destination con checkpoints independientes.
9. CAROUSEL mantiene siempre seis posiciones físicas.
10. Con 0..1 DISTRIBUTED se usa un único carousel de seis posiciones.
11. Con 2+ DISTRIBUTED, posiciones 1..5 forman el scheduler normal y posición 6 tiene scheduler DISTRIBUTED independiente.
12. QUEUE_IN_QUEUE mantiene schedulers independientes MINE/PLANT, con 4/5 components respectivamente y 3 slots visibles por área.
13. Recovery del Modeler reconstruye el presente desde state+checkpoint+replay; no reproduce obligatoriamente todas las rotaciones atrasadas.
14. No crear adapters/mirror types sólo para leer Tool metadata externa.

## Open conflicts

- Materialization CURRENT todavía repite semantic validation que el target asigna a Command Center pre-publicación.
- Materialization CURRENT todavía depende de Web/projection packages.
- `DeliveryAlarmConfiguration` CURRENT importa `ToolConfigurationKind`, incompatible con el target de Engine sin Tool-model dependency.
- Canonical histórico que describe Delivery como consumidor directo de Runtime sigue siendo cierto de la implementación, pero no del target.
- `13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md` decía que Delivery receiver “no es proceso Engine”; esa formulación debe leerse como descripción del receiver CURRENT, no como prohibición de un Delivery target dentro del Alarm Engine.
- Tool contract duplication `ada.web.tools.*` vs `ada.contracts.tools.*` bloquea el qualifier Web y permanece fuera de este frente.
