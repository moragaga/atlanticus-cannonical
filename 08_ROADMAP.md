# Atlanticus — Roadmap

Estado: **CANONICAL BASELINE 1.0 — EXECUTION IN PROGRESS**

## Regla

Cerrar verticalmente capacidades integrables.

Un solo foco por incremento.

No conservar legacy por conveniencia de tests.

## Checkpoint publicado de referencia

```text
moragaga/atlanticus@a065f45c55a527c96ce333705465487e95f0a737
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
```

## Users closure

El clean cutover removió la compatibilidad schema v1 que bloqueaba el cierre.

No permanecen en runtime CURRENT:

```text
schema_v1.py
decode_users_profiles_schema_v1(...)
Source schema-v1 fallback
Projection schema-v1 fallback
```

Qualification posterior:

```text
ruff scoped
PASS

pytest scoped
99 passed

forbidden scan
PASS / zero matches sobre código CURRENT

full Web pytest
545 passed
7 skipped
0 failed

git diff --check HEAD^..HEAD
PASS

working tree
CLEAN
```

## Siguiente foco único

```text
TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED / NEXT
```

Secuencia:

1. inspeccionar ownership y rutas reales de Tools;
2. identificar contratos Source/Projection y composición Manager actuales;
3. contrastar contra `ManagerModule` CURRENT;
4. clasificar cualquier diferencia;
5. recomendar una opción concreta;
6. implementar sólo después de consenso;
7. ejecutar qualification scoped y global cuando corresponda.

No asumir que Tools requiere arquitectura especial ni copiar mecánicamente el cambio de Users.

## Después de Tools

```text
KPI-CONFIG-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED

KPI-DEFINITION-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED
```

No abrirlos junto con Tools.

## Qualification global vigente

```text
545 passed
7 skipped
0 failed
```

## No mezclar en el siguiente chat

- KPI Configuration;
- KPI Definition;
- Python migration;
- Docker E2E general;
- ADA-specific work;
- rediseño de Manager core.

Único foco:

```text
TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER
```
