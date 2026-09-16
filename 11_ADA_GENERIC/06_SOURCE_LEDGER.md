# ADA Generic — Source Ledger

Estado: **AUDIT LEDGER**

## Autoridad vigente

```text
Implementation
moragaga/atlanticus:main

Current inspected checkpoint
27c2e4beed125fe379881048f0df5fbe3ff6cb1a

Canonical
moragaga/atlanticus-cannonical:main

Historical
moragaga/atlanticus-decisions:main
```

## Referencias históricas

Entre las fuentes históricas existentes:

- `Atlanticus_ADA_Autoridad_Tool_KPI_Render_Alarmas_2026-09-01.docx`
- `Atlanticus_ADA_Composition_Incremento_2026-09-01.docx`
- `Atlanticus_Materializacion_Runtime_Aplicacion_y_Plan_Continuidad_2026-09-01.docx`
- `Atlanticus_WEB-COMPOSITION-001_Contrato_Composicion_Modular_2026-09-01.docx`
- `ATLANTICUS_MANAGER_GLOBAL_RULES_2026-09-02.md`
- Alarm decisions indicadas por canonical cuando corresponda.

Estas fuentes son HISTORICAL y no reemplazan implementación/canonical CURRENT.

## Implementación relevante inspeccionada

```text
scopes/ada/web/tools/configuration
scopes/ada/web/application/ada-configuration-manager
web/capabilities/source/core
web/capabilities/projection/core
```

## Tools checkpoint

`27c2e4be...` publica:

```text
ToolSourceService
ToolSourceCodec
ToolSourceRelease
ToolProjectionBuilder
create_tool_projection_service(...)
```

Y elimina del dominio Tools el lifecycle/source/projection privado anterior.

## Ownership confirmado

Tools sigue siendo ADA-specific:

```text
scopes/ada/web/tools
```

El uso de Source/Projection genéricos no mueve la capability al core Atlanticus.

## Desalineación temporal del consumer

`ada-configuration-manager` todavía referencia el contrato Tool anterior.

No se crea compatibilidad para resolverlo anticipadamente.

El cutover del consumer se hará después de migrar KPI Configuration y KPI Definition.

## Regla

Cuando histórico, canonical e implementación difieren:

- `atlanticus:main` define realidad implementada;
- canonical define contrato/estado vigente y debe actualizarse;
- historical puede explicar rationale;
- no reintroducir código removido por una referencia histórica.

## Siguiente frontera

```text
KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
PLANNED / NEXT
```

Usar Tools CURRENT como referencia estructural y verificar primero el código KPI real.
