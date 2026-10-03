# ADA Command Center — Open Items

Estado: **CURRENT — capability parity CLOSED; Alarm Engine extraction design NEXT; full Web qualifier BLOCKED separately**.

## CLOSED / VERIFIED

```text
ada-contracts-tools package
ada-contracts-alarms package
Command Center Alarm/Tool consumer cutover in main
shared Alarm schema ownership in ada-contracts-alarms

Command Center Users parity
Command Center Profiles parity
Command Center Navigation parity
Command Center Manager principal/runtime parity
Command Center Users recovery integration
Command Center Master Users special operation wiring
four Web lockfile normalization
```

Qualification focal:

```text
configuration-manager   31 passed
generic-application     12 passed
catalog-manager          8 passed
Ruff                    PASS
```

## BLOCKED — separate qualifier issue

El qualifier ya no se detiene en Configuration Manager.

Ahora falla en:

```text
web/tools/catalog
web/tools/discovery-cosmos
```

Causa observada:

```text
ada.contracts.tools types
!=
ada.web.tools types
```

No corregir ADA dentro de este foco y no introducir adapters en Command Center.

## NEXT único

```text
ADA-ALARM-ENGINE-EXTRACTION-DESIGN
```

Alcance:

```text
todo scopes/ada-command-center/backend como candidato Engine
dependency inventory
backend -> Web dependency removal/inversion
Command Center publication -> Engine input contract
destination of domain/alarms
target package/scope graph
incremental move plan
```

No implementar hasta congelar diseño.

## OPEN / SEPARATE

```text
resolution of ADA Tools contract duplication
production Entra
Docker/Azure final runtime
Alarm Live Projection
History/Analytics
final distribution
product-specific branding
Python 3.14.7/Trixie
```
