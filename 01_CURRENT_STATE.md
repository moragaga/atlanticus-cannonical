# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

## Autoridad

Implementación publicada al inicio de este cierre:

```text
moragaga/atlanticus@55cd6121e000a6af5d4f0dc0ea2e384f97a27f2a
```

El usuario mantiene un working tree local posterior con cambios no publicados.

Por regla de autoridad:

- `atlanticus:main` continúa siendo la realidad implementada publicada;
- el working tree local es evidencia del incremento en progreso;
- canonical no debe declarar como CURRENT un cambio local todavía no publicado.

## Estado resumido

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER          CLOSED / VERIFIED / CURRENT
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER           CLOSED / VERIFIED / CURRENT
USERS-MANAGER-GENERIC-CONTRACT-CUTOVER             CLOSED / VERIFIED / CURRENT

USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL        IN PROGRESS
USERS-CLEAN-CUTOVER-COMPLETION                     PLANNED / NEXT

PROJECTION-CORE-STALE-TEST-ALIGNMENT               CLOSED / VERIFIED

TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER             PLANNED
KPI-CONFIG-MANAGER-GENERIC-CONSUMER-CUTOVER        PLANNED
KPI-DEFINITION-MANAGER-GENERIC-CONSUMER-CUTOVER    PLANNED
```

## VERIFIED durante este hito

### Manager / Users Manager

`users-manager` fue alineado al contrato genérico de Manager.

La ruta `Exact*` anterior dejó de ser la frontera de composición.

El contrato vigente permanece:

```text
ManagerModule
├── source_key
├── source_service
├── source_reader_service
├── projection_service
├── draft_validation_service
└── source_history_service | None
```

### Users Configuration local cutover

En el working tree local se eliminaron o reemplazaron piezas de la arquitectura revision-based, incluyendo:

```text
bundle.py
contracts.py
services.py
projection.py
runtime_projection.py
validation.py
configuration adapters legacy
old web callbacks/layout
expected_source_revision
base_source_revision executable contract
projection_source_revision
revision -> ProjectionTarget reconstruction
```

También se removieron los modelos paralelos de authoring:

```text
UsersConfigurationCatalog
UserProfileConfiguration
```

### Qualification observada

Qualification scoped de Users:

```text
ruff: PASS
pytest: 113 passed
git diff --check: PASS
```

Qualification global Web después de alinear un test stale de Projection core:

```text
pytest: 546 passed, 7 skipped
```

El test stale esperaba un mensaje anterior referido sólo a release; producción valida correctamente el `ProjectionTarget` completo.

## VERIFIED problema pendiente

El mismo working tree introdujo compatibilidad permanente con un schema viejo:

```text
web/capabilities/users/configuration/.../schema_v1.py
decode_users_profiles_schema_v1(...)
```

y branches de lectura schema v1 en Source/Projection.

Eso constituye compatibilidad legacy aunque:

- no se llame `Adapter`;
- sea read-only;
- ayude a leer historia durable;
- mantenga los tests verdes.

Por tanto:

```text
USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL
IN PROGRESS
```

No está CLOSED.

## DECIDED / FROZEN

```text
LEGACY
REMOVE

ADAPTERS / SHIMS / ALIASES
FORBIDDEN

DOBLE CONTRATO
FORBIDDEN

OLD SCHEMAS IN RUNTIME CODE
REMOVE

expected_source_revision
REMOVE

revision -> ProjectionTarget reconstruction
REMOVE

CONTRATO FINAL
Generic Atlanticus contract only
```

No se permite una excepción implícita para “durable history compatibility”.

Si existe información real persistida en formato viejo, su migración debe resolverse explícitamente como operación de migración, no como código de compatibilidad permanente dentro del contrato CURRENT.

## Refinamiento del orden de trabajo

La secuencia vigente es:

```text
1. completar migración/cutover
2. borrar legacy completamente
3. borrar tests que sólo preservan legacy
4. ejecutar qualification scoped
5. ejecutar qualification global
6. adjudicar desalineaciones reales restantes
```

No se modifica producción para hacer pasar tests que defienden contratos removidos.

## SUPERSEDED

Queda reemplazada la decisión introducida durante este chat de conservar schema-v1 read compatibility dentro del runtime.

```text
schema-v1 compatibility in CURRENT runtime
SUPERSEDED / REMOVE
```

También queda reemplazada cualquier conclusión previa que marcara Users como CLOSED sólo porque la suite estaba GREEN.

## INFERRED

La suite GREEN demuestra consistencia del working tree con los tests existentes, pero **no demuestra cumplimiento arquitectónico** cuando esos tests aceptan o prueban compatibilidad expresamente prohibida.

## ASSUMED

Ninguno de los formatos schema v1 debe conservarse en runtime por defecto.

Si existe una necesidad operacional real de migrar datos históricos, debe comprobarse desde datos/entorno autoritativo antes de diseñar una migración puntual.

## UNVERIFIED

- eliminación completa de `schema_v1.py`;
- eliminación de todos los branches schema v1;
- ausencia total de old schema readers en el resto de `web`;
- qualification global después de esa eliminación;
- full ADA suite;
- Docker E2E;
- CI remoto;
- Python 3.14.7/Trixie global;
- estado de Tools/KPI respecto del contrato Manager genérico.

## Siguiente frontera

```text
USERS-CLEAN-CUTOVER-COMPLETION
```

No abrir Tools/KPI hasta cerrarla.
