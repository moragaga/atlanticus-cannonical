# ADA Command Center — Tool Catalog

Estado: **CURRENT V1 backend/Storage y Manager B1d integrado en host temporal / CLOSED qualification local; DECIDED/PLANNED extracción Web y Starter**. Corte adicional 2026-09-29.

## Backend owner

```text
scopes/ada-command-center/backend/tools/catalog
```

## Contract

```text
ToolCatalogEntry
    tool_key
    display_name
    kind
    source_release_id
    structure

ToolCatalogSnapshot
    revision
    generated_at_utc
    tools
```

Revision is deterministic from normalized Tool entries.

## Consolidation

The package consumes configured Tool ProjectionStore inputs and produces an all-or-nothing snapshot.

A failed refresh does not overwrite current Storage state.

## Durable output

```text
ToolCatalogStore
BlobToolCatalogStore
```

V1 persists one CURRENT catalog in Storage/Blob.

No catalog history or retention is implemented here.

## Command Center topology

Operational integration may observe multiple upstream Tool Cosmos/projection surfaces plus prior
Storage state during reconciliation/certification.

Confirmed output:

```text
Confirmed Tool Catalog -> Storage
```

It intentionally does not project back into Command Center Cosmos.

## Consumer — Alarm Configuration

One exact snapshot produces:

```text
AlarmToolReferenceCatalog
    catalog_revision
    UI tools
    dependencies: ToolDependencyManifest
```

UI tools omit STRATEGIC.
Dependency catalog preserves all snapshot entries.

## Shared downstream Tool contract

Owner:

```text
scopes/ada-command-center/domain/tools
```

```text
ToolDependencyEntry
ToolDependencyManifest
```

Shared by Alarm publication/history and backend Materialization.

## History strategy

Tool Catalog Store still keeps only CURRENT.

Historical exact Tool evidence required by an Alarm revision is persisted in:

```text
AlarmConfigurationSnapshot.tool_dependencies
```

Therefore B.2 does not require versioned historical lookup from `BlobToolCatalogStore`.

## Authoring behavior

`AlarmConfiguration` remains Rules + Messages.

But Alarm Manager publication is no longer externally unrestricted:
- Save Draft requires current Confirmed Tool Catalog;
- validation requires referenced keys in pinned revision;
- publish rejects revision drift.

This refines the old "catalog is only optional authoring assistance" statement.

## Integración Manager B1d — CURRENT; ownership Web objetivo — PLANNED

**CURRENT en `atlanticus:main@caced5d7711cf059d36ec61aecc9b3e9629bd41f`:**

```text
backend/tools/catalog/                         # snapshot/codec/Blob store/consolidación
backend/tools/discovery-cosmos/                 # discovery nombrado y ToolCatalogManagerService
web/application/ada-command-center-configuration-manager/
  src/.../catalog_manager.py                   # UI y callbacks B1d, aún ligados al host
  src/.../composition.py                       # ManagerEntry y prefijo /manager
  src/.../local_runtime.py | durable_runtime.py  # adapters/providers por composición
web/alarms/configuration/                      # capability Web independiente
```

El `pages/manager.py` del host registra el contenedor `/manager`; Tool Catalog se monta como `ManagerEntry(route='/tool-catalog')`. **No** es una página autónoma ni una librería Web reutilizable todavía. Discovery usa conexiones Cosmos externas con nombre; confirmación re-inspecciona y exige fingerprint/revisión previa iguales para detectar drift. El catálogo CURRENT permanece en Blob y no se proyecta a Command Center Cosmos.

**VERIFIED en entorno de cualificación local del usuario:** 38 tests backend, 31 Web Manager, 6 qualification; Sources/Projections Cosmos independientes y `READY` para dos conexiones; confirmación manual y `verify-catalog` de Blob, con revisión de ejemplo `6a26feedc3cf7cee4ebcf5a93ad59314180635875ab25423bb576a052e517243`. El caso de prueba no es preloading productivo; las Tool keys y releases usados no definen estructura fija del producto. Azure productivo/CI/distribución permanecen **UNVERIFIED**.

**DECIDED target / PLANNED implementation:** extraer la interfaz y sus callbacks a una biblioteca `scopes/ada-command-center/web/tools/...`, independiente tanto del Configuration Manager actual como del futuro Starter. Exponer una composición pública de Manager equivalente en independencia a la de `web/alarms/configuration`; la aplicación aporta el servicio de inspección/confirmación/adopción, principal/autorización y configuración de conexiones. El módulo Web no debe adquirir conexiones globales, decidir providers, importar `__main__` del host ni duplicar `backend/tools/catalog`/`discovery-cosmos`.

Después de extraer: el host actual consume la nueva biblioteca, se elimina su `catalog_manager.py` anterior sin adaptadores de compatibilidad, y un Starter `ada-command-center-generic` compone Tool Catalog más Alarm Configuration para distribución. Identidad, perfiles, usuarios, navegación y futuras capacidades son **opt-in**, no paquetes obligatorios por anticipación. Este diseño no altera Domain Tools ni la evidencia histórica Rn/Cn. La revisión visual y normalización de pages son posteriores, sujetas a aceptación visual.

## Non-goals

- Tool authoring in Command Center;
- consolidated Tool Cosmos output;
- B.2 inside Tool Catalog;
- Tool catalog history solely for Alarm materialization.
