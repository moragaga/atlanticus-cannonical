# Atlanticus — Roadmap

Estado: **CANONICAL BASELINE 1.0 — EXECUTION IN PROGRESS**

## Regla

Cerrar contratos raíz de forma limpia y luego integrar consumers.

Un solo foco por incremento.

No conservar legacy para mantener consumers o tests anteriores funcionando.

## Checkpoint publicado de referencia

```text
moragaga/atlanticus@27c2e4beed125fe379881048f0df5fbe3ff6cb1a
```

## Hitos cerrados

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-MANAGER-GENERIC-CONTRACT-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-CLEAN-CUTOVER-COMPLETION
CLOSED / VERIFIED / CURRENT

USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL
CLOSED / VERIFIED / CURRENT

PROJECTION-CORE-STALE-TEST-ALIGNMENT
CLOSED / VERIFIED

TOOLS-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## Tools closure

Tools continúa en:

```text
scopes/ada/web/tools
```

El dominio no fue generalizado.

Su infraestructura CURRENT usa:

```text
ToolSourceService
SourceStore
SourceSnapshot
SourceReleaseRef
ToolProjectionBuilder
ProjectionTarget
ProjectionStore[ToolConfiguration]
SourceProjectionService[ToolConfiguration]
```

El lifecycle/source/projection privado anterior fue removido del paquete Tools.

`ada-configuration-manager` permanece temporalmente desalineado y no debe repararse con compatibilidad.

## Siguiente foco único

```text
KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
PLANNED / NEXT
```

Secuencia:

1. inspeccionar únicamente `scopes/ada/web/kpis/configuration`;
2. identificar contratos Source/Projection privados CURRENT;
3. identificar exactamente cómo consume Tool Projection;
4. preservar semántica KPI y ownership ADA;
5. sustituir identidad revision-oriented por contratos Source/Projection genéricos;
6. representar dependencia Tool mediante identidad genérica de Projection;
7. eliminar legacy del dominio;
8. no tocar todavía `ada-configuration-manager`;
9. no abrir KPI Definition en el mismo incremento.

Usar Tools CURRENT como patrón estructural, no como plantilla ciega.

## Después de KPI Configuration

```text
KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
PLANNED
```

Después:

```text
ADA-CONFIGURATION-MANAGER-FINAL-CUTOVER
BLOCKED until KPI domains are migrated
```

Después:

```text
MANAGER-CONSUMER-GLOBAL-QUALIFICATION
BLOCKED until final Manager cutover
```

## Qualification

Para Tools:

```text
contract/code inspection
VERIFIED

scoped test execution
UNVERIFIED
```

La regresión global se ejecutará después del cutover final del Configuration Manager.

## No mezclar en el siguiente chat

- KPI Definition;
- final Configuration Manager cutover;
- Python migration;
- Docker E2E general;
- Command Center;
- Operational Data;
- rediseño de Manager core;
- parches para imports legacy del consumer.

Único foco:

```text
KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
```
