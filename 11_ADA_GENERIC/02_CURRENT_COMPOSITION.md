# ADA Generic — Current Composition

Estado: **VERIFIED**

Implementación auditada:

```text
scopes/ada/web/application/ada-generic-application
moragaga/atlanticus@d484569cbe0290f38f239481cde81b13a23deecf
```

## Actualmente compone/consume

Entre otras capacidades:

- branding;
- ADA navigation;
- ADA operational header;
- alarm management summary;
- alarm status;
- content state;
- operational render binding;
- operational state;
- runtime experience;
- source consumption / operational participation;
- time status;
- global indicators;
- session/runtime Web.

## Generic Application y Collector

La Generic Application sigue siendo válida sin Collector.

El cierre de Collector **no** añadió `ada-web-kpi-collector` como dependencia obligatoria de
`ada-generic-application`.

El contrato implementado para composición externa es:

```text
create_application_definition(...)
        ↓
attach_ada_kpi_collector(definition, collector)
        ↓
create_web_application(...)
```

Esto conserva la frontera:

```text
Generic Application
→ generic composition

Operational composition root
→ resolves Tool/Cosmos
→ optionally attaches Collector
```

## Runtime

`create_application_runtime` sigue reuniendo la composición Web genérica y estados de consumo
operacional en un `WebApplicationRuntime` sin requerir Collector.

Qualification posterior al cierre Collector:

```text
scripts/scopes/ada/check.sh application
63 passed
commented mirrors PASS
```

## Siguiente integración

No está verificado todavía el montaje del collector con una Tool operacional real.

```text
ADA-GENERIC-COLLECTOR-OPERATIONAL-INTEGRATION
PLANNED / NEXT
```

El próximo incremento debe localizar el composition root real y aplicar el attachment allí. No
hardcodear Tool/Cosmos dentro de Generic Application.
