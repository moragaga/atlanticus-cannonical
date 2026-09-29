# ADA Command Center — Tool Catalog

Estado: **CURRENT V1 / C1 WEB OWNERSHIP IMPLEMENTED & STRUCTURALLY CLOSED / B1d LOCAL TOOL QUALIFICATION HISTORICAL; Starter, Azure y aceptación browser UNVERIFIED**. Checkpoint implementado: `atlanticus:main@3961385aecd0eb7e373018fc25e509a71dccc409` (2026-09-29).

## Ownership CURRENT después de C1

```text
scopes/ada-command-center/web/tools/catalog
    ada-command-center-web-tool-catalog==0.1.0
scopes/ada-command-center/web/tools/discovery-cosmos
    ada-command-center-web-tool-discovery-cosmos==0.1.0
scopes/ada-command-center/web/tools/catalog-manager
    ada-command-center-web-tool-catalog-manager==0.1.0
```

Los antiguos paquetes `backend/tools/catalog`, `backend/tools/discovery-cosmos`, namespaces `ada_command_center.tools.*` y nombres de distribución anteriores están **SUPERSEDED / ausentes del árbol Git actual**. No usar wrappers de compatibilidad. El hecho de ejecutar lógica Python del lado servidor no implica ownership Backend si sirve exclusivamente a Web. Contratos genuinamente transversales permanecen en `domain/tools`.

## Contract — CURRENT

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

Revision determinista derivada de Tool entries normalizadas; entradas de Tool ordenadas/unívocas según su contrato implementado.

## Consolidation y discovery — CURRENT

`web/tools/catalog` consume entradas Tool ProjectionStore configuradas y genera snapshot all-or-nothing. Un refresh fallido **no** sobrescribe Storage CURRENT. `web/tools/discovery-cosmos` descubre/inspecciona por conexiones Cosmos externas nombradas, exige revisión/fingerprint para detectar drift, confirma y aporta `ToolCatalogManagerService`. No convertir reconocimiento de proyecciones externas en preloading productivo inferido.

## Durable output — CURRENT

```text
ToolCatalogStore
BlobToolCatalogStore
Confirmed Tool Catalog -> Blob CURRENT
```

Existe un único catálogo CURRENT reemplazable en Storage/Blob V1. NO existe historia interna del catálogo ni proyección de salida consolidada a Command Center Cosmos. El nombre del contenedor Blob puede venir de `.env` del host; su blob y namespace se derivan de la composición actual de la aplicación. Las referencias Cosmos físicas de otros contratos no se configuran mediante nombres de contenedor en `.env`.

## Consumer — Alarm Configuration / manifest exacto

Una única revisión leída produce:

```text
AlarmToolReferenceCatalog
    catalog_revision
    UI tools
    dependencies: ToolDependencyManifest
```

Las UI Tool references omiten STRATEGIC; el `ToolDependencyManifest` conserva todas las entries necesarias para el snapshot. Contrato compartido:

```text
scopes/ada-command-center/domain/tools
ToolDependencyEntry
ToolDependencyManifest
```

Alarm publicación/historia congela en `AlarmConfigurationSnapshot.tool_dependencies` el subconjunto exacto del catálogo Cn asociado a Rn. B.2 no necesita buscar una versión histórica del Blob Catalog, ni consultar Cosmos Tool latest para reinterpretar una release Alarm antigua.

## Authoring behavior — CURRENT

`AlarmConfiguration` conserva Rules + Messages. `Save Draft` exige catálogo confirmado CURRENT; validación verifica claves referenciadas bajo la revisión fijada; publicación rechaza revision drift y congela `ToolDependencyManifest(Cn)`. Queda SUPERSEDED la formulación antigua «catálogo opcional para authoring». No alterar estas reglas en la normalización de rutas futura.

## Manager integration — CURRENT tras B1d y C1

```text
web/application/ada-command-center-configuration-manager/
    composition.py                        # integra ManagerEntry/host
    local_runtime.py | durable_runtime.py # compone clients y providers
web/tools/catalog-manager/
    manager.py                            # UI y callbacks capability-local
web/tools/discovery-cosmos/
    manager.py                            # ToolCatalogManagerService
web/tools/catalog/                      # consolidación, Blob y codec
web/alarms/configuration/              # authoring independente
```

El host temporal registra `/manager` y monta Tool Catalog como Manager Entry `/tool-catalog`; no se convierte por ello en página autónoma ni en Starter productivo. La biblioteca UI expone una fábrica pública `create_tool_catalog_manager_entry(manager, principal_provider, group_key='configuration')` y no importa el host. La aplicación decide conexiones, provider/principal y Store; Domain no importa aplicación Web temporal.

**VERIFIED por Git y usuario en C1:** commit remoto C1 `3961385a...`; código, espejos, tests y namespaces bajo Web; locks actualizados; Tool Catalog 13 tests, Discovery 38, Catalog Manager 8, host 27 y regresiones Alarm Web/Backend reportadas; Ruff/format GREEN tras cambios limitados a imports; wheels de `web_tool_catalog` y `web_tool_discovery_cosmos` construidos; importaciones del host de los tres paquetes nuevas PASS. No equiparar tests/importación con prueba de navegador ni distribución aislada.

**HISTORICAL B1d local qualification:** 38 tests backend, 31 Web Manager y 6 qualification del corte B1d, Cosmos Emulator + Azurite con dos conexiones READY, confirmación manual y verificación del Blob de revisión `6a26feedc3cf7cee4ebcf5a93ad59314180635875ab25423bb576a052e517243`. Aquellas Tool keys y releases fueron fixtures de qualification, no topología productiva fija. No declarar Alarm Source/Projection durable E2E probado a partir de este gate.

## Starter objetivo — PLANNED / no implementado por C1

`ada-command-center-generic` deberá componer Tool Catalog y Alarm Configuration desde bibliotecas independientes, sólo después de acordar ubicación física/entrypoint y ejecutar gates propios. Identidad, navegación, usuarios y perfiles se integran cuando su contrato real lo requiera; no crear un segundo dominio ni acoplarse a `ada-generic-application`. El host temporal podrá retirarse cuando el Starter cubra y valide sus responsabilidades; NO hacerlo en C1.

## Non-goals

- Authoring de Tools en Command Center.
- Salida Tool Catalog consolidada a Command Center Cosmos.
- B.2/qualification dentro de Tool Catalog.
- Historia de Tool Catalog sólo para Materialization.
- Live/History/Analytics o cualificación Azure/Docker inferidos por C1.
