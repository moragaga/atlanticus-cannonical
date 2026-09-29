# ADA Command Center — Configuration Scope

Estado: **CURRENT / ALARM CONFIGURATION SNAPSHOT V3 IMPLEMENTED; C1 WEB TOOL OWNERSHIP CLOSED / aceptación browser y Source/Projection durable UNVERIFIED**. C1 `atlanticus:main@3961385aecd0eb7e373018fc25e509a71dccc409`.

Command Center owns Alarm Configuration administration and reuses `atlanticus.web.manager`. Generic Manager remains unchanged.

## Editable aggregate

```text
AlarmConfiguration
    rules
    messages
```

El editor no embebe Tool definitions.

## Durable published aggregate

```text
AlarmConfigurationSnapshot
    configuration: AlarmConfiguration
    tool_dependencies: ToolDependencyManifest
```

Es el payload durable CURRENT de Alarm Source schema v3. La formulación anterior según la cual Tool metadata no modificaba el snapshot durable está **SUPERSEDED**.

## Tool correlation

Metadata específica del workspace:

```text
_confirmed_tool_catalog_revision
```

No forma parte de `AlarmConfiguration`.

## Save / validate / publish

```text
Save Draft
-> read current Confirmed Tool Catalog
-> pin Cn in workspace

Validate
-> intrinsic AlarmConfiguration validation
-> require pinned Cn == current Cn
-> require every referenced Tool key exists

Verify Source
-> generic Manager source concurrency

Publish
-> require Cn still current
-> select referenced ToolDependencyEntries
-> persist AlarmConfigurationSnapshot v3
```

No es obligatoria una caché de validación en memoria. Un workspace guardado C1 no se publica silenciosamente si Tools avanza a C2; release histórica R1/C1 permanece inmutable.

## Tool references persistidas

Comprenden Tool origins, todos los pasos de escalamiento (incluidos disabled), todos los visual targets y todas las Rules (incluidas inactive).

## Authoring UI y C1

El read model expone sugerencias Tool/Component/Subcomponent. STRATEGIC no es elegible para visualización Alarm, pero el manifest completo derivado del mismo snapshot conserva todos los tipos Tool; `routing_tools` y visual `tools` son superficies distintas de la implementación.

El catálogo Cn se origina hoy en `web/tools/catalog`, se descubre/confirma mediante `web/tools/discovery-cosmos` y la UI propia reside en `web/tools/catalog-manager`; el host temporal sólo los compone. La afirmación B1d «UI de Tool Catalog requiere extracción» está **SUPERSEDED** por C1. Su **revisión visual** sigue OPEN/SEPARATE; ninguna suite automatizada certifica aceptación visual final.

## Projection base

```text
Source release
-> AlarmConfigurationProjectionBuilder
-> ProjectionRecord[AlarmConfigurationSnapshot]
```

No relectura posterior de latest Tool Catalog para una release Rn/Cn ya publicada.

## Evidencia histórica B1d y límite de aceptación

La qualification B1d publicó/proyectó Tools por contratos ADA existentes y confirmó un catálogo consumido por el Manager. Se observó una Alarm Source en filesystem `local`, **no** publicación/proyección física durable de Alarm Source con provider `durable`. La publicación y la proyección son acciones separadas; cambiar `local -> durable` no implica migración automática. El guard Cn y Source v3 permanecen intactos tras C1.

OPEN/SEPARATE: semántica de máximo de desactivación, incluido fin del turno, y cierre del modal de Alarm Configuration únicamente después de guardado exitoso. No inferir que la Source local acredita estas correcciones.

## Adapters y siguiente frontera

Existen adapters local/Cosmos y composición Source local/Blob y Projection local/Cosmos en código, observado ya en el corte histórico `atlanticus@7b61eaea463bab10a595166fa12d015e4c015c78`; esto **reemplaza** afirmaciones antiguas sobre inexistencia de adapter Cosmos, pero no acredita Azure E2E.

**Siguiente foco de este traspaso C2:** normalizar la identidad/rutas y el contrato de contenedor Cosmos del job Materialization junto con Runtime y Delivery, manteniendo Blob container ambiental. C1 no autorizó modificar el editor ni sus contratos de negocio; revisión UX corresponde a otro foco.
