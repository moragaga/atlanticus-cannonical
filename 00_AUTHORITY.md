# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Checkpoint CURRENT verificado para este cierre:
  `4fe03660ad47d105c55167dc583f09be1f395275`
- Parent inmediato:
  `8e133ad7da3524874add8323315e8c2b3c3f1ee1`
- Tree:
  `d893128c23939e8a1e8bdd60b5bae64d14983be8`
- Fecha del commit:
  `2026-09-21T18:52:46Z`

Estado acumulado relevante:

```text
ADA-STORAGE-NAMESPACE                         CLOSED / VERIFIED / CURRENT
TOOL-PROJECTION-PERSISTENCE                   CLOSED / VERIFIED / CURRENT
TOOL-PERSISTENCE-RESILIENT-COMPOSITION        CLOSED / VERIFIED / CURRENT

ADA-WEB-KPI-COLLECTOR-CAPABILITY              CLOSED / VERIFIED / CURRENT
ADA-GENERIC-OPERATIONAL-BOOTSTRAP             CLOSED / VERIFIED / CURRENT
ADA-GENERIC-COLLECTOR-RUNTIME-WIRING          CLOSED / VERIFIED / CURRENT
ADA-GENERIC-OPERATIONAL-RENDER-RUNTIME-CONTRACT
                                               CLOSED / VERIFIED / CURRENT
ADA-GENERIC-STAGE-1                           CLOSED / VERIFIED / CURRENT

COMMAND-CENTER-WEB-ALARM-CONFIGURATION-CONTRACT
                                               CLOSED / VERIFIED / CURRENT
COMMAND-CENTER-ALARM-CONFIGURATION-PROJECTION CLOSED / VERIFIED / CURRENT
COMMAND-CENTER-ALARM-CONFIGURATION-MANAGER-INTEGRATION
                                               CLOSED / VERIFIED / CURRENT
COMMAND-CENTER-TOOL-CATALOG-V1                CLOSED / VERIFIED / CURRENT
COMMAND-CENTER-ALARM-TOOL-REFERENCES-V1       CLOSED / CURRENT
```

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Checkpoint inspeccionado antes de este reemplazo:
  `fb5b000d0f38a535fca606fe01a324cb0f6185b4`

`atlanticus-cannonical:main` es autoridad documental vigente, subordinada a
`atlanticus:main` cuando la implementación publicada demuestra un estado posterior.

### Historical decisions

- Repositorio: `moragaga/atlanticus-decisions`
- Rama: `main`
- Checkpoint observado:
  `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`

Permanece **HISTORICAL**.

Las decisiones B.1/B.2 preservan semántica útil de AlarmDefinition, Live/Management y
Runtime/Delivery, pero no prevalecen sobre la implementación CURRENT cuando describen:

- SharePoint como autoridad física de dominios ya migrados a Source/Release + Blob;
- una pre-save validation externa estricta que impide persistir referencias Tool/evaluator todavía
  no resolubles;
- un Confirmed Tool Catalog con estados de reconciliación no implementados en V1.

CURRENT conserva:

```text
VALID != FULLY RESOLVED != READY
UNRESOLVED != INVALID
```

## Jerarquía

1. `atlanticus:main`: realidad implementada.
2. `atlanticus-cannonical:main`: contratos, fronteras, roadmap y estado vigente.
3. Qualification/tests vigentes: evidencia de propiedades demostradas.
4. Decisiones explícitas del Project todavía no formalizadas en canonical: delta temporal.
5. `atlanticus-decisions`: referencia histórica.
6. Memoria/historial conversacional: pista, nunca autoridad suficiente.

## Clasificación obligatoria

```text
VERIFIED
INFERRED
ASSUMED
PROPOSED
UNVERIFIED
```

Estados:

```text
CURRENT
IN PROGRESS
PLANNED
SUPERSEDED
BLOCKED
CLOSED
```

Si implementación y canonical se contradicen, exponer el conflicto y actualizar canonical;
nunca retroceder implementación CURRENT para satisfacer documentación obsoleta.

## Git

Git es **READ ONLY** por defecto.

No crear commits, push, ramas, PR, issues ni mutaciones remotas sin autorización explícita.

## Continuidad congelada

No reabrir sin conflicto demostrado:

```text
LEGACY
REMOVE

ADAPTERS / SHIMS / ALIASES
FORBIDDEN

DOUBLE CONTRACT
FORBIDDEN

OLD SCHEMA RUNTIME READERS
FORBIDDEN

revision -> ProjectionTarget reconstruction
REMOVE

expected_source_revision
REMOVE
```

## Storage namespace CURRENT

ADA dispone de:

```text
AdaStorageNamespace
application_namespace
tool_namespace
```

Separación congelada:

```text
physical container / connection
!=
application namespace
!=
tool namespace
!=
SourceKey
```

## Tool Projection CURRENT

Persistencia durable:

```text
LocalToolProjectionStore
CosmosToolProjectionStore
```

Runtime activo:

```text
resolve_active_tool_projection()
→ durable Tool Projection
```

Source sólo participa en workflows de materialización/update:

```text
project_current_tool_source()
```

## Tool persistence composition CURRENT

Providers independientes:

```text
Source     = local | blob
Projection = local | cosmos
```

Estados de resolución de una Tool individual:

```text
READY
UNCONFIGURED
UNAVAILABLE
INVALID
```

Estos estados pertenecen a Tool persistence y no se copiaron al Command Center Tool Catalog V1.

## ADA Generic Stage 1 CURRENT

Cadena implementada:

```text
environment / .env
→ provider settings
→ AdaStorageNamespace
→ ToolPersistenceComposition
→ resolve_active_tool_projection()
→ ToolStructure
→ AdaKpiCollector
→ Latest Delivery / Timeseries Delivery
→ process cache
→ browser dcc.Store por ToolComponent
→ frontera de consumo del desarrollador
```

La frontera de render permanece estructural y no se reabre desde Command Center.

## ADA Command Center Alarm Configuration CURRENT

Implementado bajo:

```text
scopes/ada-command-center/web/alarms/configuration
```

Cadena durable CURRENT:

```text
Manager Workspace
→ intrinsic validation
→ Alarm Configuration Source/Release
→ Alarm Configuration Projection
```

La unidad publicada es atómica:

```text
Alarm Rules + Message Catalog
```

La Projection base conserva la Alarm Configuration de una `SourceRelease` exacta y no introduce
Tool Catalog, evaluator resolution, B.2, Runtime ni Delivery.

## Command Center Tool Catalog V1 CURRENT

Implementado bajo:

```text
scopes/ada-command-center/backend/tools/catalog
```

Frontera:

```text
named Tool Projection inputs
→ ToolCatalogConsolidator
→ ToolCatalogSnapshot
→ ToolCatalogStore
→ Blob CURRENT
```

Invariantes CURRENT:

- ADA Tool Configuration continúa siendo owner de Tool authoring/topology;
- Command Center Tool Catalog es read-only derived state;
- `tool_key` es identidad y debe ser único dentro del snapshot;
- no existe Cosmos propio de Command Center para duplicar Tool topology;
- el consolidator no publica snapshot parcial;
- si cualquier input falla, falta o es inválido, `replace_current()` no se ejecuta;
- Blob CURRENT conserva naturalmente el último snapshot publicado correctamente;
- V1 no implementa estados AVAILABLE/STALE/MISSING, history, scheduler ni LKG separado;
- container y blob name son configuración explícita del `BlobToolCatalogStore`.

## Alarm Tool References V1 CURRENT

Alarm Configuration dispone de un read model backend-only:

```text
ToolCatalogStore
→ AlarmToolReferenceReader
→ AlarmToolReferenceCatalog
```

Expone Tools, Components y Subcomponents utilizables por authoring sin consultar los Cosmos
individuales.

La ausencia legítima de catálogo se representa como `None`.

Errores del store no se convierten silenciosamente en catálogo vacío.

`STRATEGIC` no se ofrece como sugerencia porque `ToolStructure` no define proyección de alarmas para
ese kind. Esto no cambia la validez intrínseca de Alarm Configuration ni prohíbe keys manuales.

## Siguiente foco único

```text
ALARM-CONFIGURATION-STRUCTURED-AUTHORING-V1
PLANNED / NEXT / DESIGN FIRST
```

Objetivo:

```text
AlarmToolReferenceCatalog
→ authoring UI Tool / Component / Subcomponent
→ mismo AlarmConfiguration durable
```

No mezclar en ese incremento:

- B.2;
- Runtime/Delivery materialization;
- Tool Catalog scheduler/cadence;
- History/Analytics;
- Command Center application shell completo.
