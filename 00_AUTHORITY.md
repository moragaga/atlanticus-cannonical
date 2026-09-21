# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Checkpoint CURRENT verificado para este cierre:
  `21cfb2f11362c1606ad14ff8adc7551948eced6a`
- Parent inmediato:
  `6155dae407dc784114ff34c7b3b6f93125432713`
- Tree:
  `48e4115a5fb53e64d83e2ae2243a9f11d612d26f`
- Fecha del commit:
  `2026-09-21T03:39:24Z`

Estado acumulado relevante para este cierre:

```text
ADA-STORAGE-NAMESPACE                         CLOSED / VERIFIED / CURRENT
TOOL-PROJECTION-PERSISTENCE                   CLOSED / VERIFIED / CURRENT
TOOL-PERSISTENCE-RESILIENT-COMPOSITION        CLOSED / VERIFIED / CURRENT

ADA-WEB-KPI-COLLECTOR-CAPABILITY              CLOSED / VERIFIED / CURRENT

ADA-GENERIC-OPERATIONAL-BOOTSTRAP             PLANNED / NEXT
```

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Checkpoint inspeccionado antes de este reemplazo:
  `4058aab3525a09b568b80f3f6a5265e45e4f6fea`

`atlanticus-cannonical:main` es autoridad documental vigente, subordinada a
`atlanticus:main` cuando la implementación publicada demuestra un estado posterior.

## Referencia histórica

`moragaga/atlanticus-decisions` permanece **HISTORICAL**.

Checkpoint observado:

```text
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

No se auditó el contenido binario de decisiones históricas durante este cierre.
Cualquier contradicción específica adicional con decisiones antiguas permanece `UNVERIFIED`.

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

Ejemplo lógico:

```text
conciencia_situacional/
├── users/
└── operaciones_integradas/
    ├── sources/
    └── projections/
```

`users` permanece global a la aplicación.

El root de Tool entregado al Source provider termina en:

```text
<application>/<tool>
```

`SourceStore` agrega internamente:

```text
sources/<SourceKey>
```

No hacer que la composición conozca ni duplique ese segmento.

## Tool Projection CURRENT

Persistencia durable implementada:

```text
LocalToolProjectionStore
CosmosToolProjectionStore
```

El documento conserva:

```text
ProjectionRecord[ToolConfiguration]
SourceKey
SourceReleaseId
source_published_at_utc
projected_at_utc
dependencies exactas
payload ToolConfiguration
```

Local:

```text
<base>/<application>/<tool>/projections
```

Cosmos:

```text
partition_key = <application>/<tool>
SourceKey     = tools
```

El namespace de deployment pertenece al store, no al contrato `SourceKey`.

## Tool persistence composition CURRENT

Capability:

```text
scopes/ada/web/tools/persistence
ada-web-tools-persistence==0.1.0
```

Providers independientes:

```text
Source     = local | blob
Projection = local | cosmos
```

Combinaciones soportadas:

```text
local + local
blob  + cosmos
blob  + local
local + cosmos
```

La construcción de `ToolPersistenceComposition` no ejecuta health check ni lectura remota.

Resolución CURRENT:

```text
READY
UNCONFIGURED
UNAVAILABLE
INVALID
```

`resolve_active_tool_projection()` lee Projection durable sin depender de Source.

`project_current_tool_source()` pertenece al flujo Source -> Projection.

## Invariante de disponibilidad

```text
APPLICATION EXISTENCE
!=
TOOL CONFIGURATION EXISTENCE
!=
EXTERNAL INFRASTRUCTURE AVAILABILITY
!=
BUSINESS DATA AVAILABILITY
```

La ausencia de Source/Projection/KPI data no debe definir la existencia del proceso Web.

Una conexión Blob/Cosmos caída debe degradar la capability afectada, no convertirse
automáticamente en caída global de la aplicación.

La integración de esta regla en el bootstrap real de ADA Generic todavía está `PLANNED`.

## Collector KPI CURRENT

El Collector permanece cerrado y CURRENT.

No rediseñar:

```text
Latest polling      = 10 s default
Timeseries polling  = 120 s default
Browser cache read  = 10 s default
Latest priority     = before Timeseries when both are due
1 ToolComponent     = 1 logical KPI Store
Subcomponent        != Store
browser             = cache only
```

## Siguiente foco único

```text
ADA-GENERIC-OPERATIONAL-BOOTSTRAP
PLANNED / NEXT
```

Debe conectar la configuración Web real con:

```text
environment/.env
→ provider settings
→ AdaStorageNamespace
→ ToolPersistenceComposition
→ resolve_active_tool_projection()
→ ADA Generic runtime/composition
```

Mantener la Web ejecutable en:

```text
READY
UNCONFIGURED
UNAVAILABLE
INVALID
```

sin reabrir Source, Projection, namespace ni Collector.

El Collector se conecta después de resolver Tool/Structure; su ausencia de datos debe seguir
siendo degradable y no una precondición de existencia de la Web.
