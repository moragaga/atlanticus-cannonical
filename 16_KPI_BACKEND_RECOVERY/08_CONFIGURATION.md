# KPI Backend Recovery — Configuration

Estado: **CURRENT for Materialization + Latest / TIMESERIES MIGRATION PLANNED**

## Named connections — CURRENT

Package:

```text
ada-kpis-connections==1.0.0
```

Contract file:

```text
config/connections.json
```

Schema:

```json
{
  "schema_version": 1,
  "connections": {
    "example_tool": {
      "endpoint_var": "EXAMPLE_TOOL_COSMOS_ENDPOINT",
      "database_var": "EXAMPLE_TOOL_COSMOS_DATABASE",
      "credential_var": "EXAMPLE_TOOL_COSMOS_KEY"
    }
  }
}
```

`connections.json` contiene nombres de variables, no valores Cosmos resueltos.

Los valores reales provienen de `.env` local o configuración/secrets del deployment.

## Tool key

```text
^[a-z][a-z0-9_]*$
```

No normalizar silenciosamente keys inválidos.

## Dynamic variable roles

Cada Tool declara exactamente:

```text
endpoint_var
database_var
credential_var
```

Las variables deben resolver a `CosmosSettings`.
La credencial es sensitive.

Implementación CURRENT rechaza dos Tools que resuelvan al mismo `(endpoint, database_name)`.

## Materialization

```text
POLL_INTERVAL_SECONDS=30
```

Connections/config se leen al iniciar el proceso.

Materialization consulta cada Tool secuencialmente.

Si el Registry document todavía no existe:

```text
readiness pending
retry = 30 s
```

Si Cosmos/config/contract falla realmente:

```text
error
```

No esconder configuración inválida detrás de retries infinitos.

## Latest Delivery

```text
POLL_INTERVAL_SECONDS=1
KPI_DELIVERY_MAX_WORKERS=2
KPI_RUNTIME_APPLICATION=<runtime application>
```

Readiness por materialized Registry:

```text
30 s
```

Ese intervalo no reemplaza el polling normal de 1 s.

## .env.detail / secrets.detail.json

CURRENT templates incluyen ejemplos explícitos para:

```text
EXAMPLE_TOOL_COSMOS_ENDPOINT
EXAMPLE_TOOL_COSMOS_DATABASE
EXAMPLE_TOOL_COSMOS_KEY
```

`config/connections.json` referencia variables y no contiene sus valores.

## Materialized Registry location

```text
<VOLUMEN_PATH>/ada-kpi-engine/materialization/registries/<tool_key>.json
```

No depende de `APPLICATION` del consumidor.

## Timeseries CURRENT legacy

Timeseries todavía usa configuración antigua:

```text
COSMOS_CONSUMPTION_ENDPOINT
COSMOS_CONSUMPTION_KEY
COSMOS_CONSUMPTION_DATABASE_NAME
KPI_TIMESERIES_DELIVERY_POLL_INTERVAL_SECONDS
```

PLANNED replacement:

```text
named connections package
materialized Registries
generic POLL_INTERVAL_SECONDS
readiness 30 s
```

No declarar esa migración como implementada antes del siguiente incremento correspondiente.
