# ADA Web — Source Ledger

Estado: **AUDIT LEDGER**

## Implementación CURRENT auditada

```text
moragaga/atlanticus@d71e94d12fa31a986b3ecc0262fbbb6ef2e4a3dd
```

### KPI Registry

```text
scopes/ada/web/kpis/registry/core
scopes/ada/web/kpis/registry/configuration
scopes/ada/web/kpis/registry/projection-local
scopes/ada/web/kpis/registry/projection-cosmos
```

Especialmente:

```text
configuration/projection_record.py
projection-cosmos/store.py
projection-cosmos/storage.py
```

### KPI Definition

```text
scopes/ada/web/kpis/definition/core
scopes/ada/web/kpis/definition/configuration
scopes/ada/web/kpis/definition/projection-local
scopes/ada/web/kpis/definition/projection-cosmos
```

Especialmente:

```text
configuration/projection_record.py
projection-cosmos/storage.py
```

### Configuration Manager

```text
scopes/ada/web/application/ada-configuration-manager
```

`local_runtime.py` demuestra durable local projections para Registry y Definition.

## Conflict ledger

Inspection:

```text
scopes/ada/web/inspection/providers/kpi-definition
```

continúa referenciando un contract histórico de Definition.

Clasificación:

```text
OPEN / SEPARATE / PREEXISTING
```
