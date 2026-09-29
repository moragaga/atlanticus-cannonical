# ADA Command Center — Tool to Alarm Configuration

Estado: **CURRENT / EXACT TOOL SNAPSHOT + STRICT ROUTING IMPLEMENTED / C1 OWNER WEB CLOSED / STRATEGIC VISUAL NOT DEFINED**. Checkpoint histórico de strict routing `atlanticus@411aea44ac60c09d2b07ce41d34c3f378788b97b`; owner C1 actual verificado `atlanticus@3961385aecd0eb7e373018fc25e509a71dccc409`. No confundir ambos cortes.

## Ownership

Tool Configuration posee `tool_key`, display name, kind, Components/Subcomponents, topology y relaciones. Alarm Configuration persiste referencias y evidencia congelada dentro de cada snapshot versionado; no redefine Tool Catalog ni duplica reconciliación.

**Tool services CURRENT tras C1:** `scopes/ada-command-center/web/tools/catalog` (consolidación + Blob), `web/tools/discovery-cosmos` (descubrimiento por conexiones nombradas) y `web/tools/catalog-manager` (UI/callbacks). La ruta histórica `backend/tools/catalog` y el ownership de UI en el host temporal quedan **SUPERSEDED**. El contrato transversal `ToolDependencyManifest` permanece en `domain/tools`.

## Topología Tool CURRENT

```text
Tool A projection/Cosmos ─┐
Tool B projection/Cosmos ─┼──> reconciliation/certification
Tool C projection/Cosmos ─┘          + prior Storage state
                                      |
                                      v
                              Confirmed Tool Catalog Cn
                                      |
                                      v
                                   Storage/Blob
                                      |
                                     END
```

La antigua salida `Confirmed Tool Catalog -> Command Center Cosmos` es **SUPERSEDED**. No reintroducirla como comodidad de un job o porque Discovery use Cosmos de entrada.

## Tool Catalog y evidencia exacta

Cada `ToolCatalogEntry` incluye `tool_key`, `display_name`, `kind`, `source_release_id` y `structure: ToolStructure`. Revisión determinista, keys ordenadas y únicas, snapshot CURRENT en Storage. `AlarmToolReferenceReader.load()` lee una única revisión confirmada y produce:

```text
visual/UI reference catalog (PROCESS + INTEGRATED_OPERATIONS)
+
full ToolDependencyManifest (incluye STRATEGIC)
```

El editor dispone además de `routing_tools` derivados del manifest completo: puede presentar Strategic como destino de routing aunque no exista proyección visual estratégica. No inventar componentes/presentación Strategic.

## Qualification B1d histórica y extracción C1

**B1d por logs del usuario:** proyecciones Cosmos de `validation_process` (release `e11cb787f51d44a5afd17febf68019a4`) y `validation_integrated` (release `1499a4f9720b47d680e91ef6faada941`), ambas discovery READY, consolidación y confirmación manual desde Manager, `verify-catalog` Blob/Azurite de revisión `6a26feedc3cf7cee4ebcf5a93ad59314180635875ab25423bb576a052e517243`. Insumos controlados; no topología física productiva permanente. La Source Alarm local observada en ese corte no verificó Alarm Source/Projection durable.

**C1 verificado:** UI ya está extraída en `web/tools/catalog-manager`; consolidación/Blob y Discovery fueron trasladados también a Web sin duplicados. Pruebas Web/Backend locales, Ruff, mirrors, ambos wheels y smoke de importaciones del host pasaron. No acredita aceptación browser visual ni Azure.

## Publish exact Rn/Cn — CURRENT

```text
current Confirmed Tool Catalog = Cn
 -> Save Draft fija Cn en sidecar `_confirmed_tool_catalog_revision`
 -> Validate exige Cn y Tool references definidas existentes
 -> Publish vuelve a comprobar Cn (drift guard)
 -> congela subset referenciado de ToolDependencyManifest(Cn)
 -> AlarmConfigurationSnapshot(Rn, Cn)
```

El subset incluye origins, todos los steps definidos (también deshabilitados) y todos los visual targets, incluso en Rules inactivas. Cada entry conserva display name y estructura históricos; `tool_dependencies.revision` determina la revisión Tool exacta. Cn+1 no reinterpreta Rn/Cn sin nueva publicación Alarm.

## Policy de dirección de routing — CURRENT / FROZEN

```text
next_routing_tool_kind(PROCESS)               = INTEGRATED_OPERATIONS
next_routing_tool_kind(INTEGRATED_OPERATIONS) = STRATEGIC
next_routing_tool_kind(STRATEGIC)             = None
```

En B.2, recorrer los pasos habilitados por `step_order`; el nivel anterior es el último escalón habilitado empezando por origin. El paso deshabilitado no crea un puente. Prohibir misma categoría, retroceso o salto. C1/C2 pueden permanecer en origen sin cambiar criticality; C3 no admite pasos habilitados.

La Web utiliza esa misma policy para opciones de destino. Configuraciones incompatibles preexistentes permanecen visibles para corrección; no eliminarlas silenciosamente.

## Frontera visual — CURRENT / LIMITADA

- Visual PROCESS: `process_projection_mode` requerido en B.2.
- Visual INTEGRATED_OPERATIONS: sin `process_projection_mode`.
- Visual STRATEGIC: B.2 bloquea porque no existe contrato visual de proyección; puede participar en routing.

**CONFLICT sin resolver:** canonical UX distingue visual target de routing target y exige acuerdo para condicionarlos; el editor actualmente sincroniza visual targets desde origin + routing habilitado no Strategic. Registrar ambos hechos, no elevar esa sincronización a contrato frozen sin decisión.

## B.2 / Qualification — frontera vigente

B.2 consume la evidencia exacta Rn/Cn del snapshot/proyección; NO relee Tool Cosmos ni latest Tool Catalog. Tool GREEN qualification hoy sigue siendo entrada explícita, sin productor operacional verificado. Su automatización verificable es C3 PLANNED. No reimplementar reconciliación del catálogo dentro de Materialization.

Manifest histórico conserva display names y estructura para explicar posteriormente routing/presentación con topología exacta de la publicación.
