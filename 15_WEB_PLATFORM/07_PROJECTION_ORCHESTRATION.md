# Web Platform — Projection Orchestration

Estado: **CURRENT / BASELINE 1.0**

## Regla principal

No existe un orden global rígido entre todas las proyecciones.

La configuración Source ya está consolidada por dominio y cada **base projection** debe poder materializarse de manera independiente.

Ejemplos:

```text
Users Source      → Users Projection
Navigation Source → Navigation Projection
Tools Source      → Tool Projection
KPI Source        → KPI Projection
Alarm Source      → Alarm Configuration Projection
```

Estas proyecciones base no deben depender unas de otras sólo para imponer un orden de bootstrap.

## Donde sí existen dependencias

Después de las base projections pueden existir **resoluciones derivadas** que consumen dos o más proyecciones ya materializadas.

Ejemplos:

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

```text
Tool Projection
      +
KPI Configuration Projection
      ↓
KPI Destination / Runtime Resolution
```

La dependencia pertenece a la resolución derivada, no a la base projection.

## Dos fases

### Phase A — Base Projections

Características:

- independientes;
- idempotentes;
- ejecutables en cualquier orden;
- no escriben en otra Source;
- publican su propio estado/revisión.

El bootstrap puede ejecutarlas secuencialmente por simplicidad, pero el orden no representa una dependencia semántica.

### Phase B — Derived Resolutions

Características:

- declaran dependencias reales;
- sólo se ejecutan cuando sus inputs están READY;
- producen materialización/resolución derivada;
- nunca escriben de regreso a las authorities base.

## Evitar ciclos

El modelo queda:

```text
SOURCE
  ↓
BASE PROJECTION
  ↓
DERIVED RESOLUTION
  ↓
RUNTIME / CONSUMER
```

Nunca:

```text
Projection A
→ modifica Source B
→ Projection B
→ modifica Source A
```

Si aparece ese ciclo, la frontera está mal definida.

## Bootstrap UI

La página de Login/Bootstrap muestra:

- Source histories/releases disponibles;
- estado de cada base projection;
- estado de cada derived resolution;
- última revisión proyectada;
- error/bloqueo;
- acción de project/reproject.

La UI no decide dependencies.

## Ejemplo de estado

```text
BASE
✓ Users
✓ Navigation
✓ Tools
✓ KPI Configuration
✓ Alarm Configuration

DERIVED
✓ Navigation/Profile Resolution
→ KPI Runtime Resolution
○ Alarm Resolution — waiting Tool revision
```

## Idempotencia

Reproyectar el mismo Source release debe producir:

```text
same effective projection
```

o un no-op equivalente.

No crear revisiones funcionales artificiales sólo por ejecutar Project nuevamente.
