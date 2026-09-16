# Atlanticus — Roadmap

Estado: **CANONICAL BASELINE 1.0 — EXECUTION IN PROGRESS**

## Regla

Cerrar contratos raíz de forma limpia y luego integrar consumers.

Un solo foco por incremento.

No conservar legacy para mantener consumers o tests anteriores funcionando.

## Checkpoint publicado de referencia

```text
moragaga/atlanticus@4c7f8aa8b541e8b8f8abc7b49fe22526a4952bfe
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

KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## KPI Configuration closure

KPI Configuration continúa en:

```text
scopes/ada/web/kpis/configuration
```

El dominio no fue generalizado.

Su infraestructura CURRENT usa Source/Projection genéricos y conserva una dependencia exacta en Tool Projection mediante `ProjectionTarget.dependencies`.

El lifecycle/source/projection privado anterior fue removido.

Qualification scoped observada:

```text
Ruff PASS
pytest 45 passed
git diff --check PASS
legacy token scan 0 matches
```

## Siguiente foco único

```text
KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
PLANNED / NEXT
```

Secuencia del siguiente chat:

1. inspeccionar únicamente `scopes/ada/web/kpis/definition` y sus dependencias contractuales directas;
2. identificar contratos Source/Projection privados CURRENT realmente existentes;
3. identificar cómo consume hoy KPI Configuration;
4. preservar semántica KPI Definition y ownership ADA;
5. contrastar con Source/Projection genéricos CURRENT;
6. definir contrato final y archivos exactos antes de editar;
7. no copiar KPI Configuration sin verificar diferencias;
8. no tocar todavía `ada-configuration-manager`;
9. no abrir Python baseline cleanup en el mismo incremento.

## Después de KPI Definition

```text
ADA-CONFIGURATION-MANAGER-FINAL-CUTOVER
BLOCKED until KPI Definition is migrated
```

Después:

```text
MANAGER-CONSUMER-GLOBAL-QUALIFICATION
BLOCKED until final Manager cutover
```

## Open independiente

Existe una desalineación de metadata Python:

```text
canonical baseline: Python 3.14.7
KPI Configuration pyproject: requires-python ==3.14.2
```

No resolverla dentro de KPI Definition salvo que el usuario abra explícitamente ese frente o impida la qualification del incremento.

## No mezclar en el siguiente chat

- final Configuration Manager cutover;
- Python baseline cleanup;
- Docker E2E general;
- Command Center;
- Operational Data;
- rediseño de Manager core;
- rediseño de Projection core;
- parches para imports legacy del consumer.

Único foco:

```text
KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
```
