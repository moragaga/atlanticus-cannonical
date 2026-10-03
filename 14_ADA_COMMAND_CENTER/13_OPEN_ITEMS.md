# ADA Command Center — Open Items

Estado: **CURRENT — capability parity NEXT; ada-contracts gate BLOCKED until parity**.

## CLOSED / VERIFIED

```text
ada-contracts-tools package
ada-contracts-alarms package
Command Center Alarm/Tool consumer cutover in main
shared schema ownership moved to ada-contracts-alarms
invalid structural/version/mirror tests removed in touched areas
qualification progressed through Web Alarm Configuration
```

## BLOCKED

### ada-contracts cutover qualification

El qualifier se detuvo en `ada-command-center-configuration-manager` por APIs viejas de Users y drift relacionado con el nuevo baseline de Users/Profiles/Navigation.

No resolverlo dentro de Alarm contracts.

## NEXT único

```text
COMMAND-CENTER-CAPABILITY-PARITY
```

Alcance:

```text
Users
Profiles
Navigation
Manager
local composition
durable composition
runtime/navigation binding
Master Projection integration where affected
functional tests
```

Reference implementation: ADA actual, sólo como patrón de composición sobre las capabilities genéricas.

## AFTER NEXT

```text
resume qualifier ada-contracts
→ full GREEN
→ inspect final diff/dependencies
→ artifact/distribution gate
```

## OPEN / SEPARATE

```text
production Entra
Docker/Azure final runtime
Alarm Live
History/Analytics
Engine physical extraction
Materialization boundary cleanup
product-specific branding
```
