# ADA Command Center — Tool to Alarm Configuration

Estado: **CURRENT / TOOL CATALOG V1 + AUTHORING READ MODEL IMPLEMENTED / B.2 OPEN**

ADA Tool Configuration es dueña de:

- `tool_key`;
- Tool kind;
- Components;
- Subcomponents;
- relaciones;
- topología.

Alarm Configuration guarda referencias.

`tool_key` es identidad estable de la Tool. `display_name` no participa de la identidad.

## Frontera CURRENT

```text
ADA Tool Configuration(s)
        ↓ durable Tool projections
Command Center Tool Catalog V1
        = consolidator / read-only derived state
        ↓
Alarm Tool Reference read model
        = authoring assistance
        ↓
Alarm Configuration
```

Command Center no inventa un segundo Tool authoring ni una segunda source of truth.

## Cadena CURRENT / PLANNED

```text
ADA Tool Configuration(s)                    CURRENT fuera de Command Center
        ↓
Tool ProjectionStore inputs                  CURRENT contract
        ↓
Command Center Tool Catalog V1               CURRENT
        ↓
AlarmToolReferenceReader                     CURRENT
        ↓
Structured Alarm Configuration authoring UI  NEXT / PLANNED
        ↓
                 + Alarm Configuration Projection   CURRENT
                 ↓
B.2 Configuration Resolution                 PLANNED
        ↓
Resolved Alarm Configuration                 PLANNED
        ↓
Runtime / Delivery capability readiness      PLANNED
```

## Tool Catalog V1

La implementación CURRENT consolida inputs explícitos `ProjectionStore[ToolConfiguration]`.

Cada entry conserva:

```text
tool_key
display_name
kind
source_release_id
ToolStructure
```

No duplica Components/Subcomponents en un contrato paralelo; reutiliza `ToolStructure`.

El snapshot:

- ordena entries por `tool_key`;
- rechaza duplicados;
- tiene revisión determinística;
- persiste como un único documento CURRENT en Blob mediante `StorageClient`;
- no introduce Cosmos propio de Command Center.

## Failure semantics V1

La reconciliación V1 es all-or-nothing.

Si cualquier input:

- falla al leer;
- no tiene Projection activa;
- no entrega `ToolConfiguration`;
- no tiene `structure`;
- repite `tool_key`;

el refresh falla antes de publicar y el CURRENT anterior queda intacto.

V1 **no** implementa estados por-entry AVAILABLE/STALE/MISSING.

La dirección canónica anterior que los exigía desde el primer contrato queda **SUPERSEDED/REFINED**
por esta primera versión funcional. Podrán agregarse sólo si una necesidad posterior lo justifica.

## Durable state CURRENT

Implementado:

```text
BlobToolCatalogStoreSettings
├── container_name
└── blob_name
```

```text
get_current()
replace_current(snapshot)
```

Blob inexistente significa que todavía no existe catálogo CURRENT y retorna `None`.

No hay todavía:

- history;
- manifest/current pointer separado;
- LKG separado;
- retention;
- scheduler/cadence.

## Alarm Tool Reference read model

`AlarmToolReferenceReader` consume el catálogo y produce una estructura de authoring con:

- Tools;
- Components;
- subcomponent addresses visibles;
- `source_release_id` por Tool;
- `catalog_revision` global.

La visibilidad de linked subcomponents se obtiene desde:

```text
ToolStructure.alarm_subcomponent_addresses_for_component()
```

No se duplica esa lógica en Command Center.

`STRATEGIC` no aparece en las sugerencias porque el contrato Tool CURRENT no define proyección de
alarmas para ese kind.

## Authoring vs resolution

El catálogo y el read model permiten authoring asistido, pero no vuelven obligatoria la resolución
externa durante persistencia.

```text
VALID CONFIGURATION
!=
FULLY RESOLVED CONFIGURATION
```

Una referencia no sugerida/no resuelta no equivale a:

- Rule inválida;
- Rule disabled;
- Rule removed.

B.2 será la frontera que resuelva referencias contra una Tool Catalog revision concreta.

## Re-resolution futura

La misma Alarm Source revision puede resolverse nuevamente contra una revisión posterior del Tool
Catalog.

```text
Alarm Source A17 + Tool Catalog T40
→ Tool unresolved

Alarm Source A17 + Tool Catalog T41
→ Tool resolved
```

La aparición de una Tool no obliga a republicar A17 si Alarm Configuration no cambió.

## Non-goals CURRENT

No crear desde este contrato:

- legacy adapters;
- aliases de compatibilidad;
- duplicación de Tool authoring;
- segunda Tool Projection Cosmos de Command Center;
- B.2 dentro del reader de authoring;
- validación externa obligatoria dentro de `AlarmConfiguration`.
