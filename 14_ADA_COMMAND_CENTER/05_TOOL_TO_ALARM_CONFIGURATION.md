# ADA Command Center — Tool to Alarm Configuration

Estado: **CURRENT / EXACT TOOL SNAPSHOT + STRICT ROUTING IMPLEMENTED / STRATEGIC VISUAL NOT DEFINED**

Checkpoint: `moragaga/atlanticus@411aea44ac60c09d2b07ce41d34c3f378788b97b`.

## Ownership

Tool Configuration posee `tool_key`, display name, kind, Components/Subcomponents, topology y relaciones. Alarm Configuration persiste referencias y evidencia congelada dentro de cada snapshot versionado; no redefine Tool Catalog ni duplica su reconciliación.

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
                                   Storage
                                      |
                                     END
```

La antigua salida `Confirmed Tool Catalog -> Command Center Cosmos` está **SUPERSEDED**. No crearla por comodidad del nuevo job.

## Tool Catalog y evidencia exacta

Owner `scopes/ada-command-center/backend/tools/catalog`.

Cada entry incluye:

```text
tool_key
display_name
kind
source_release_id
structure: ToolStructure
```

El catálogo es de revisión determinista, keys ordenadas/únicas y snapshot CURRENT en Storage. `AlarmToolReferenceReader.load()` consume una única revisión confirmada y produce:

```text
visual/UI reference catalog (PROCESS + INTEGRATED_OPERATIONS)
+
full ToolDependencyManifest (incluye STRATEGIC)
```

El editor tiene además `routing_tools` derivados del manifest completo: puede ofrecer Strategic como destino de routing aunque no tenga contrato visual. No usar esa lista para inventar componentes/presentación Strategic.

## B1d — evidencia física local y frontera Web pendiente

**VERIFIED por salida local reportada:** proyecciones Cosmos de `validation_process` (release `e11cb787f51d44a5afd17febf68019a4`) y `validation_integrated` (release `1499a4f9720b47d680e91ef6faada941`); discovery `READY` en ambas conexiones, consolidación manual desde Manager y `verify-catalog` con revisión `6a26feedc3cf7cee4ebcf5a93ad59314180635875ab25423bb576a052e517243` en Storage/Azurite. Son **insumos controlados de qualification**, no registros productivos ni un requisito de que siempre existan esas dos Tools.

El código de UI del catálogo está en la aplicación temporal, a diferencia de la capability separada Alarm Configuration; su extracción a `web/tools/...` está **DECIDED como objetivo y PLANNED en código**. Este cambio de ownership Web no modifica el catálogo persistido ni la evidencia congelada Rn/Cn. La Source local observada de Alarm Configuration no verifica aún la publicación ni la proyección Cosmos durable de alarmas.

## Publish exact Rn/Cn — CURRENT

```text
current Confirmed Tool Catalog = Cn
 -> Save Draft fija Cn en sidecar `_confirmed_tool_catalog_revision`
 -> Validate exige Cn y todas las Tool references definidas existentes
 -> Publish vuelve a comprobar Cn (drift guard)
 -> congela subset referenciado de ToolDependencyManifest(Cn)
 -> AlarmConfigurationSnapshot(Rn, Cn)
```

El subset incluye origins, **todos** los steps definidos (también deshabilitados) y todos los visual targets, aun en Rules inactivas. Cada entry conserva nombre y estructura históricos; `tool_dependencies.revision` determina la revisión Tool exacta. La revisión Tool Cn+1 posterior no reinterpreta Rn/Cn sin nueva publicación Alarm.

## Policy de dirección de routing — CURRENT / FROZEN

```text
next_routing_tool_kind(PROCESS)               = INTEGRATED_OPERATIONS
next_routing_tool_kind(INTEGRATED_OPERATIONS) = STRATEGIC
next_routing_tool_kind(STRATEGIC)             = None
```

En B.2, los pasos **habilitados** se recorren por `step_order`; el nivel anterior es el último escalón habilitado, comenzando por origin. Un paso deshabilitado no crea puente para saltarse niveles. Prohibidos rutas mismo nivel, retrocesos y saltos. C1/C2 pueden quedarse en origen sin alterar criticidad; C3 no admite pasos habilitados.

En la Web, las opciones de destinos utilizan exactamente esa policy compartida. Configuraciones preexistentes incompatibles permanecen visibles para su corrección; no eliminarlas silenciosamente.

## Frontera visual — CURRENT / LIMITADA

- Visual `PROCESS`: `process_projection_mode` requerido por B.2.
- Visual `INTEGRATED_OPERATIONS`: sin `process_projection_mode`.
- Visual `STRATEGIC`: B.2 lo bloquea porque no existe contrato de proyección visual. Strategic sólo participa en routing.

**CONFLICT A DOCUMENTAR:** canonical de UX dice que visual target y routing target no son equivalentes y no deben condicionarse sin acuerdo explícito. La implementación del editor sincroniza visual targets desde origin + routing habilitado no Strategic. Mantener ambos hechos visibles; no asumir que la sincronización automática congela un nuevo contrato.

## B.2 y job siguiente

B.2 consume la evidencia exacta del snapshot/proyección Rn/Cn, no Tool Cosmos ni latest Tool Catalog. Tool GREEN qualification continúa como input explícito cuyo productor operacional está OPEN. El siguiente job debe recuperar el snapshot íntegro y esa qualification sin reimplementar reconciliación.

El manifest histórico conserva `display_name` y estructura para explicar posteriormente routing y presentación con los nombres/topología vigentes cuando se publicó la alarma.
