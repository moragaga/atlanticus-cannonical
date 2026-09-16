# Web Platform — Projection Orchestration

Estado: **CURRENT / BASELINE 1.0 REFINED BY IMPLEMENTATION**

## Regla principal

No existe un orden global rígido entre todas las proyecciones.

Una projection no debe depender de otra sólo para imponer un orden de bootstrap.

Cuando existe una dependencia semántica real que forma parte de la identidad exacta de la projection, se declara mediante:

```text
ProjectionTarget.dependencies
```

## Base projections sin dependencia

Pueden materializarse de forma independiente cuando su contrato no requiere otra projection.

Ejemplos:

```text
Users Source      → Users Projection
Navigation Source → Navigation Projection
Tools Source      → Tool Projection
```

## Projections con dependencia semántica real

KPI Configuration es el caso CURRENT implementado:

```text
Tool ProjectionTarget
        ↓ exact dependency
KPI Configuration ProjectionTarget
```

La KPI Configuration Projection no relee simplemente el Tool CURRENT durante la ejecución.

El target seleccionado conserva el Tool ProjectionTarget exacto. Si el snapshot Tool disponible cambia antes de construir KPI Configuration, la ejecución falla en vez de proyectar contra una dependencia diferente.

Dirección congelada para el siguiente dominio:

```text
KPI Configuration ProjectionTarget
        ↓ exact dependency
KPI Definition ProjectionTarget
```

La implementación concreta de KPI Definition permanece PLANNED / NEXT y debe inspeccionarse antes de editar.

## Derived resolutions

Las resoluciones derivadas siguen siendo válidas cuando consumen projections ya materializadas para construir una vista o materialización que no forma parte de la identidad de una de esas projections.

Ejemplos existentes de dirección arquitectónica:

```text
Users Projection
      +
Navigation Projection
      ↓
optional Profile/Navigation Resolution
```

```text
Tool Projection
      +
Alarm Configuration Projection
      ↓
Resolved Alarm Configuration
```

No mover una dependencia semántica real a una derived resolution sólo para mantener artificialmente independientes las base projections.

## Identidad y ordering

`ProjectionTarget.dependencies`:

- contiene `ProjectionTarget` completos;
- prohíbe dependencia sobre el mismo `source_key`;
- prohíbe source keys duplicadas;
- se normaliza de forma determinista por `source_key`.

La dependencia no se representa con revision strings privadas.

## Evitar ciclos

Nunca:

```text
Projection A
→ modifica Source B
→ Projection B
→ modifica Source A
```

Si aparece ese ciclo, la frontera está mal definida.

## Bootstrap / Manager

La UI o el coordinator pueden decidir cuándo ofrecer/ejecutar acciones, pero no inventan la identidad de dependencia.

La dependencia pertenece al `ProjectionTarget` del dominio correspondiente.

## Idempotencia

Reproyectar el mismo exact target debe conservar la misma identidad efectiva o producir un no-op equivalente según el contrato Projection CURRENT.

No crear revisiones funcionales artificiales sólo por ejecutar Project nuevamente.

## Refinamiento de Baseline 1.0

La formulación histórica:

```text
all base projections are independent
real dependencies only exist in derived resolutions
```

queda SUPERSEDED como regla universal.

La regla CURRENT es:

```text
no artificial bootstrap dependencies
+
exact ProjectionTarget.dependencies for real semantic projection dependencies
+
derived resolutions only for genuinely derived composition
```
