# Atlanticus — Open Questions

Estado: **CANONICAL OPEN ITEMS**

Los puntos aquí no reabren contratos CLOSED.

## CLOSED — Manager generic core

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

No están OPEN:

- doble routing exact/legacy;
- `workflow_service` como lifecycle Manager;
- `ExactSource*` como frontera Manager;
- `ExactProjectionWorkflow` como frontera Manager;
- `expected_source_revision`;
- reconstruction revision→`ProjectionTarget`;
- shims/adapters de compatibilidad Manager.

## CLOSED — Navigation

```text
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## CLOSED — Users Manager consumer

```text
USERS-MANAGER-GENERIC-CONTRACT-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## CLOSED — Users clean cutover

```text
USERS-CLEAN-CUTOVER-COMPLETION
CLOSED / VERIFIED / CURRENT

USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL
CLOSED / VERIFIED / CURRENT
```

Ya no están OPEN:

```text
schema_v1.py
decode_users_profiles_schema_v1(...)
schema-v1 read compatibility in Source
schema-v1 read compatibility in Projection
```

La adjudicación `REMOVE` fue implementada y verificada.

Si existiera necesidad real de migrar datos viejos, sería un trabajo operacional explícito y separado, sujeto a evidencia real.

## OPEN — Tools consumer

```text
TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED / NEXT
```

Sigue OPEN porque durante este cierre no se inspeccionaron ownership, rutas, contratos Source/Projection ni composición Manager de Tools.

Preguntas permitidas:

1. ¿Dónde vive exactamente Tools en la implementación CURRENT?
2. ¿Qué contratos Source/Projection usa?
3. ¿Cómo construye su `ManagerModule`?
4. ¿Existe alguno de los elementos Manager SUPERSEDED?
5. ¿Hay desviación real o ya cumple el contrato genérico?
6. Si hay desviación, ¿cuál es el incremento mínimo y verificable?

No asumir respuestas antes de inspeccionar `atlanticus:main`.

## OPEN — KPI Configuration consumer

```text
KPI-CONFIG-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED
```

No analizar junto con Tools.

## OPEN — KPI Definition consumer

```text
KPI-DEFINITION-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED
```

No analizar junto con Tools.

## Qualification

Users cerró con:

```text
ruff scoped
PASS

pytest scoped
99 passed

full Web pytest
545 passed
7 skipped
0 failed

git diff --check HEAD^..HEAD
PASS

working tree
CLEAN
```

## UNVERIFIED

- full ADA suite;
- Docker E2E;
- CI remoto;
- Python 3.14.7/Trixie global;
- Tools consumer;
- KPI Configuration consumer;
- KPI Definition consumer;
- existencia de datos históricos schema v1 que requieran migración operacional.

## Siguiente foco

```text
TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER
```

Fuentes obligatorias:

```text
moragaga/atlanticus:main
moragaga/atlanticus-cannonical:main
```

Git sólo lectura para el asistente.
