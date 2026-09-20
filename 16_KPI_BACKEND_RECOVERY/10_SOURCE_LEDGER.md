# KPI Backend Recovery — Source Ledger

Estado: **AUDIT LEDGER**

Corte CURRENT:

```text
moragaga/atlanticus@d71e94d12fa31a986b3ecc0262fbbb6ef2e4a3dd
```

## KPI Runtime

Inspeccionado:

```text
scopes/ada/backend/processes/kpi-runtime/src/ada/processes/kpi_runtime/job.py
settings.py
composition.py
```

Verificado:

```text
observed == committed → up_to_date skip
no REPROCESS_CURRENT setting
KpiPersistence commit remains fenced
```

## Historian

Inspeccionado:

```text
scopes/ada/backend/processes/kpi-historian/src/ada/processes/kpi_historian/job.py
settings.py
```

Verificado:

```text
historian current → SKIPPED_CURRENT
normal read_after(after, through)
no REPROCESS_CURRENT setting
```

## Delivery

Inspeccionado:

```text
scopes/ada/backend/processes/kpi-delivery/src/ada/processes/kpi_delivery/configuration.py
settings.py
job.py
```

Verificado consumer CURRENT:

```text
document_type = ada_kpi_configuration_projection
payload = configuration
binding identity = key
```

Settings CURRENT:

```text
KPI_DELIVERY_CONFIGURATION_CONTAINER
KPI_DELIVERY_CONFIGURATION_ITEM_ID
KPI_DELIVERY_CONFIGURATION_PARTITION_KEY
```

## Timeseries Delivery

Inspeccionado:

```text
scopes/ada/backend/processes/kpi-timeseries-delivery/src/ada/processes/kpi_timeseries_delivery/configuration.py
settings.py
job.py
```

Verificado el mismo legacy configuration projection contract.

## KPI Registry Web CURRENT

Inspeccionado:

```text
scopes/ada/web/kpis/registry/configuration/.../projection_record.py
scopes/ada/web/kpis/registry/projection-cosmos/.../store.py
scopes/ada/web/kpis/registry/projection-cosmos/.../storage.py
```

Verificado:

```text
document_type = ada_kpi_registry_projection_record
schema_version = 1
SourceKey = kpis
payload = KpiRegistry
payload.bindings[].kpi_key
logical_id = ada.kpis.registry.projection
physical = ada-kpi-registry-projection
partition path = /partition_key
TTL = None
```

## KPI Definition Web CURRENT

Inspeccionado:

```text
scopes/ada/web/kpis/definition/configuration/.../projection_record.py
scopes/ada/web/kpis/definition/projection-cosmos/.../storage.py
```

Verificado:

```text
document_type = ada_kpi_definition_projection_record
ProjectionRecord[KpiDefinitionCatalog]
logical_id = ada.kpis.definition.projection
physical = ada-kpi-definition-projection
```

## Conflict

Backend Delivery/Timeseries todavía no consumen el Registry Web CURRENT.

Clasificación:

```text
VERIFIED / OPEN
```
