# Alarm Engine — Decision Index

Estado: **CURRENT canonical decisions at `atlanticus@6725237...`**.

Este archivo resume decisiones vigentes de este frente. `atlanticus-decisions` es referencia histórica, no autoridad actual por defecto.

| Frontera | Estado |
|---|---|
| Business Alarm model | CURRENT / frozen. |
| Source snapshot + exact Tool manifest | CURRENT. |
| Shared Tool contracts | CURRENT en `ada-contracts-tools`. |
| Shared Alarm contracts | CURRENT en `ada-contracts-alarms`. |
| `domain/tools` | SUPERSEDED / removed. |
| `domain/alarms` shared model ownership | SUPERSEDED; queda source key + routing policy. |
| Published valid configuration | CURRENT concept: `AlarmConfigurationSnapshot`. |
| Public `ResolvedAlarmConfiguration` stage | SUPERSEDED. |
| Materialization deterministic splitter | DECIDED / PLANNED implementation cleanup. |
| READY/EFFECTIVE exact pin | CURRENT. |
| Engine CURRENT v1 / FACTS v2 | CURRENT. |
| Delivery input CURRENT-only | CURRENT. |
| Engine physical extraction | PLANNED / separate. |
| Full Live Delivery / History / Analytics | PLANNED / separate. |

## Refinamientos cerrados en este hito

1. Los modelos Alarm/Tool que cruzan productos ya no pertenecen a packages de Command Center.
2. Los schemas de publicación Engine pertenecen a `ada-contracts-alarms`.
3. Command Center puede depender de los contracts; los contracts no dependen de ADA ni Command Center.
4. `ToolDependencyManifest` sigue siendo parte del snapshot publicado.
5. El stage adicional `ResolvedAlarmConfiguration` no forma parte del target publicado.
6. Materialization no debe ser owner de validación semántica que Command Center ya cerró antes de publicar.
7. El gate de packaging/build reemplaza tests que sólo congelaban metadata o forma interna.

## Conflictos / deuda preservada

- Materialization CURRENT aún conserva responsabilidades/acoplamientos previos que contradicen el target limpio; no se corrigió en este incremento.
- Command Center Configuration Manager está desalineado frente a Users/Profiles/Navigation CURRENT; bloquea el qualifier.
- Production Entra y runtime Azure siguen UNVERIFIED.
- Engine físico sigue bajo Command Center aunque la dirección futura considera `ada-alarm-engine`; no mover hasta cerrar la frontera técnica.
