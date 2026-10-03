# Alarm Engine — Open Items

Estado: **CURRENT — no abrir Engine durante el próximo incremento**.

## CLOSED / CURRENT

```text
shared Tool contracts package
shared Alarm contracts package
Engine publication schemas ownership
Command Center consumer cutover implementado en main
invalid structural/version/mirror tests removed in touched scopes
partial qualification through Web Alarm Configuration
```

## BLOCKED

### Full ada-contracts cutover gate

Bloqueado en Command Center Configuration Manager por drift de Users/Profiles/Navigation/Manager frente al baseline genérico actual.

Tratamiento autorizado:

```text
resolver primero Command Center parity
luego retomar exactamente el qualifier
```

No parchear los contracts de Alarm para compensar ese drift.

## PLANNED / AFTER GATE

- audit final de dependencies y packages;
- distribución final;
- ejecución Engine/Delivery desde artefactos distribuidos;
- Docker independiente;
- cleanup de Materialization para hacerlo splitter determinista;
- eventual extracción física `ada-alarm-engine` si sigue justificada.

## OPEN / SEPARATE

- Live materialization/AlarmLiveProjection;
- Management Capture;
- History/Analytics;
- production Entra;
- Azure/CI;
- datasets reales y Tool/evaluator qualification;
- migraciones físicas sólo si existe histórico real que las requiera.

## Invariantes que no se reabren

```text
contracts before consumers
READY != EFFECTIVE
exact artifact pin
Runtime and Delivery same exact artifact
no fallback to latest READY
no legacy adapters by inference
no Tool semantic rediscovery in target Materialization
```
