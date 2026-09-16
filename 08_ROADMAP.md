# Atlanticus — Roadmap

Estado: **CANONICAL BASELINE 1.0 — EXECUTION IN PROGRESS**

## Regla

No expandir arquitectura general sin necesidad de producto.

Cerrar verticalmente capacidades integrables y verificables.

Mantener un foco por incremento.

## Checkpoint actual

```text
moragaga/atlanticus@d34cda3838a67907728b382e238f0178f9f1a64e
```

## Hitos cerrados

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Navigation ya no pertenece a la lista de consumers pendientes.

## Resultado Navigation

- una sola ruta Source/Projection;
- legacy Navigation Configuration removido;
- adapters/shims/aliases de transición prohibidos;
- `expected_source_revision` removido;
- reconstruction revision→`ProjectionTarget` removida;
- Local Source + Local Projection implementado;
- Blob Source + Cosmos Projection compuesto para Azure;
- Ruff scoped PASS;
- tests scoped `102 passed`;
- forbidden legacy scan `0 results`.

## Siguiente foco

La suite global reveló una desalineación preexistente en `users-manager`.

No se abre todavía un cutover de Users.

Primero:

```text
USERS-MANAGER-ALIGNMENT-VALIDATION
PLANNED / NEXT
```

Objetivo:

1. inspeccionar `web/compositions/users-manager` en `atlanticus:main`;
2. contrastar con el contrato Manager CURRENT;
3. revisar canonical vigente;
4. enumerar archivos y contratos desalineados;
5. adjudicar qué es CURRENT, SUPERSEDED o legacy;
6. decidir sólo después si existe un incremento de implementación.

## Consumers todavía no revalidados

```text
TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED

KPI-CONFIG-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED

KPI-DEFINITION-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED
```

No se presupone que requieran los mismos cambios.

## Qualification global

```text
MANAGER-CONSUMER-GLOBAL-QUALIFICATION
BLOCKED
```

Bloqueo vigente:

```text
users-manager stale Exact* contract
solution not yet adjudicated
```

Después de resolver ese bloqueo y los consumers que correspondan, repetir qualification global.

## No mezclar en el siguiente chat

- Navigation;
- Tools;
- KPI Configuration;
- KPI Definition;
- Python migration;
- Docker E2E general;
- ADA-specific work;
- rediseño de Manager core.

Único foco: Users Manager alignment validation.
