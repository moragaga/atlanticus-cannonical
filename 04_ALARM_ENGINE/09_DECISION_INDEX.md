# Alarm Engine — Decision Index

Estado: **CURRENT at `atlanticus@346e7ac7...`**.

| Frontera | Estado |
|---|---|
| Business Alarm model | CURRENT / frozen. |
| Source snapshot + exact Tool manifest | CURRENT. |
| Shared Tool contracts | CURRENT in `ada-contracts-tools`. |
| Shared Alarm contracts | CURRENT in `ada-contracts-alarms`. |
| `domain/tools` | SUPERSEDED / removed. |
| Published valid configuration | CURRENT: `AlarmConfigurationSnapshot`. |
| Public `ResolvedAlarmConfiguration` stage | SUPERSEDED. |
| Materialization deterministic splitter | CURRENT target / implementation cleanup pending. |
| READY/EFFECTIVE exact pin | CURRENT. |
| Engine CURRENT v1 / FACTS v2 | CURRENT. |
| Delivery input CURRENT-only + FACTS | CURRENT. |
| Command Center capability parity | CLOSED. |
| Engine physical extraction | PLANNED / NEXT DESIGN. |
| Live Projection / History / Analytics | PLANNED / separate. |

## Refinements from this closure

1. Command Center capability parity no longer blocks Alarm/contract qualification.
2. Full Web qualification is now blocked separately by duplicated Tool contract types upstream.
3. Do not modify ADA as part of the Command Center or Alarm Engine extraction closure.
4. The next Engine design starts with the full `scopes/ada-command-center/backend` as candidate ownership, not only `alarms/core`.
5. Dependencies from that backend toward Command Center Web are debt to REMOVE/INVERT, not dependencies to preserve.
6. Physical extraction to `scopes/ada-alarm-engine` requires a frozen dependency graph before implementation.
7. `ada-command-center/domain/alarms` must be reclassified explicitly rather than dragged across by path.

## Open conflicts

- backend Materialization still depends on Web packages;
- physical package/module names still use `ada_command_center`;
- historical documentation that states the Engine necessarily belongs to Command Center is SUPERSEDED as a target direction but remains true of current physical location;
- Tool contract duplication blocks the full Web qualifier and is outside this next focus.
