# KPI Backend Recovery — Configuration

Estado: **CURRENT**

## REPROCESS_CURRENT

Disponible sólo en:

```text
kpi-runtime
kpi-historian
```

Default:

```text
false
```

## Cosmos connection

External configuration:

```text
COSMOS_CONSUMPTION_ENDPOINT
COSMOS_CONSUMPTION_KEY
COSMOS_CONSUMPTION_DATABASE_NAME
```

## Container contract

No ENV para:

```text
container name
item id
partition key path
partition value
TTL
document_type
schema_version
```

Esos valores son contratos internos del proceso.

Consumed Registry:

```text
validate/read only
never provision
```

Owned Delivery/Timeseries output:

```text
ensure once at startup
never per iteration
```

La database permanece externa y no es creada por estos procesos.
