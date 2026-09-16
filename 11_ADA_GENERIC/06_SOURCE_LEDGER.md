# ADA Generic — Source Ledger

Estado: **AUDIT LEDGER**

## Autoridad vigente

```text
Implementation
moragaga/atlanticus:main

Current inspected checkpoint
ef3f0a44c5dcc14f8fcafe5bb36bb97865381924

Canonical
moragaga/atlanticus-cannonical:main

Canonical checkpoint inspected before replacement
430a90529c99e69d16978f60d91aa86f819b851e

Historical
moragaga/atlanticus-decisions:main
```

## Referencias históricas

Las fuentes históricas existentes permanecen HISTORICAL y no reemplazan implementación/canonical CURRENT.

## Implementación relevante inspeccionada

```text
scopes/ada/web/tools/configuration
scopes/ada/web/kpis/configuration
scopes/ada/web/kpis/definition
scopes/ada/web/application/ada-configuration-manager
web/capabilities/source/core
web/capabilities/projection/core
web/capabilities/manager
web/compositions/users-manager
```

## Tools

```text
TOOLS-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Ownership:

```text
scopes/ada/web/tools
```

## KPI Configuration

```text
KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Checkpoint:

```text
4c7f8aa8b541e8b8f8abc7b49fe22526a4952bfe
```

Dependencia exacta CURRENT:

```text
Tool ProjectionTarget
→ KPI Configuration ProjectionTarget.dependencies
```

## KPI Definition

```text
KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Checkpoint:

```text
ef3f0a44c5dcc14f8fcafe5bb36bb97865381924
```

CURRENT:

```text
KpiDefinitionSourceService
KpiDefinitionProjectionBuilder
KpiDefinitionCatalog
ProjectionStore[KpiConfiguration]
exact KPI Configuration ProjectionTarget dependency
```

No CURRENT:

```text
KpiDefinitionAuthorityCatalog
KpiDefinitionAuthorityProvider
KpiDefinitionServices
private revision lifecycle
expected_source_revision
```

## Ownership confirmado

Tools, KPI Configuration y KPI Definition siguen siendo ADA-specific bajo `scopes/ada`.

El uso de Source/Projection genéricos no mueve esas capabilities al core Atlanticus.

## Desalineación temporal del consumer

`ada-configuration-manager` todavía referencia contratos anteriores de Users/Navigation/Tools/KPI.

No se crea compatibilidad en los dominios para resolverlo.

Todos los dominios Configuration requeridos ya están migrados, por lo que el consumer final deja de estar bloqueado por un dominio pendiente.

## Regla

Cuando histórico, canonical e implementación difieren:

- `atlanticus:main` define realidad implementada;
- canonical define contrato/estado vigente y debe actualizarse;
- historical puede explicar rationale;
- no reintroducir código removido por una referencia histórica.

## Siguiente frontera

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
PLANNED / NEXT
```

Inspeccionar primero el consumer CURRENT y construir el delta exacto contra contratos ya publicados. No inventar APIs ni reintroducir legacy.
