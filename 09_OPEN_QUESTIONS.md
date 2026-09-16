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

No reabrir Navigation para resolver Users.

## CLOSED — Users Manager consumer

```text
USERS-MANAGER-GENERIC-CONTRACT-CUTOVER
CLOSED / VERIFIED / CURRENT
```

La composition de Users consume el contrato genérico Manager.

## OPEN — Users clean cutover completion

```text
USERS-CLEAN-CUTOVER-COMPLETION
PLANNED / NEXT
```

VERIFIED problema:

```text
schema_v1.py
decode_users_profiles_schema_v1(...)
schema-v1 read compatibility in Source
schema-v1 read compatibility in Projection
```

Adjudicación ya decidida:

```text
REMOVE
```

No está OPEN decidir si conservarlo.

Está OPEN únicamente ejecutar su eliminación y verificar consecuencias del contrato final.

### Preguntas operativas permitidas

1. ¿En qué archivos exactos queda todavía interpretación de schema viejo?
2. ¿Qué tests existen únicamente para esa compatibilidad?
3. ¿Qué otros símbolos/fallbacks equivalentes no fueron incluidos en el scan anterior?
4. ¿La suite CURRENT queda verde después de eliminar todo legacy?
5. Si falla, ¿el fallo pertenece al contrato final o a una expectativa SUPERSEDED?

### No son preguntas abiertas

- si schema v1 debe conservarse;
- si una lectura read-only merece excepción;
- si historia durable justifica fallback permanente;
- si hay que agregar adapter/shim;
- si los tests obligan a conservar comportamiento viejo.

Todo eso está decidido: **NO**.

## OPEN — Tools consumer

```text
TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED
```

No analizar junto con Users.

## OPEN — KPI Configuration consumer

```text
KPI-CONFIG-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED
```

## OPEN — KPI Definition consumer

```text
KPI-DEFINITION-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED
```

## Qualification

La full Web suite local llegó a:

```text
546 passed
7 skipped
```

pero debe repetirse después del clean cutover final.

## UNVERIFIED

- absence total de old schema readers después del próximo incremento;
- full Web GREEN post-cleanup;
- full ADA suite;
- Docker E2E;
- CI remoto;
- Python 3.14.7/Trixie global;
- impacto de Tools/KPI.

## Siguiente foco

```text
USERS-CLEAN-CUTOVER-COMPLETION
```

Fuentes obligatorias:

```text
moragaga/atlanticus:main
moragaga/atlanticus-cannonical:main
```

Git sólo lectura para el asistente.
