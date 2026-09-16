# Atlanticus — Roadmap

Estado: **CANONICAL BASELINE 1.0 — EXECUTION IN PROGRESS**

## Regla

Cerrar verticalmente capacidades integrables.

Un solo foco por incremento.

No conservar legacy por conveniencia de tests.

## Checkpoint publicado de referencia

```text
moragaga/atlanticus@55cd6121e000a6af5d4f0dc0ea2e384f97a27f2a
```

El cutover de Users se encuentra en un working tree local posterior todavía no publicable como CURRENT.

## Hitos cerrados

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-MANAGER-GENERIC-CONTRACT-CUTOVER
CLOSED / VERIFIED / CURRENT

PROJECTION-CORE-STALE-TEST-ALIGNMENT
CLOSED / VERIFIED
```

## Hito en progreso

```text
USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL
IN PROGRESS
```

Se implementó una parte importante del cutover, pero el cierre fue invalidado al detectar compatibilidad schema v1 en runtime.

## Decisión de clean cutover

Todo lo siguiente debe desaparecer antes de cerrar Users:

```text
schema_v1.py
decode_users_profiles_schema_v1(...)
Source schema-v1 fallback
Projection schema-v1 fallback
tests dedicados únicamente a esos fallbacks
cualquier adapter/shim/alias equivalente
```

No se agrega una segunda ruta para mantener historia o tests.

## Siguiente foco único

```text
USERS-CLEAN-CUTOVER-COMPLETION
PLANNED / NEXT
```

Secuencia:

1. inspeccionar exactamente todos los lugares que todavía entienden schema/contrato viejo;
2. eliminarlos;
3. no agregar reemplazos de compatibilidad;
4. corregir/eliminar tests que prueben exclusivamente legacy;
5. ejecutar scan completo;
6. ejecutar Ruff y pytest scoped;
7. ejecutar full Web pytest;
8. sólo entonces declarar Users CLOSED.

## Después de Users

Consumers pendientes:

```text
TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED

KPI-CONFIG-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED

KPI-DEFINITION-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED
```

No se presupone que requieran los mismos cambios.

No comenzar ninguno hasta cerrar Users.

## Qualification global

Observada en el working tree actual:

```text
546 passed
7 skipped
```

Esto prueba que el árbol actual es consistente con sus tests, pero **no cierra Users** mientras exista compatibilidad prohibida.

La próxima qualification relevante es la posterior a la eliminación final de legacy.

## No mezclar en el siguiente chat

- Tools;
- KPI Configuration;
- KPI Definition;
- Python migration;
- Docker E2E general;
- ADA-specific work;
- rediseño de Manager core.

Único foco:

```text
USERS-CLEAN-CUTOVER-COMPLETION
```
