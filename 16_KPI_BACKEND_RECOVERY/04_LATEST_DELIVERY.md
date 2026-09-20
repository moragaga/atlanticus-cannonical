# KPI Backend Recovery — Latest Delivery

Estado: **CLOSED / VERIFIED / CURRENT**

## Registry input

Delivery ya no consume `ada_kpi_configuration_projection`.

CURRENT:

```text
container       = ada-kpi-registry-projection
partition path  = /partition_key
partition value = kpis
document_type   = ada_kpi_registry_projection_record
schema_version  = 1
```

Reader independiente del proceso; no import `ada.web`, no shared reader package, no fallback legacy.

`source_release_id` se usa como `configuration.revision`; la dependency Tool aporta `tool_projection_revision`.

## Output owned

```text
container       = ada-kpi-latest-delivery
partition path  = /partition_id
TTL             = None
id              = latest
partition_id    = kpis
document_type   = ada_kpi_latest_delivery
schema_version  = 1
```

Startup:

```text
validate Registry container
ensure Latest output container
read Registry
freeze effective configuration
execute job
```

No provisioning per iteration.

Qualification:

```text
processes/kpi-delivery/tests 32 passed
kpis/delivery/tests          28 passed
```
