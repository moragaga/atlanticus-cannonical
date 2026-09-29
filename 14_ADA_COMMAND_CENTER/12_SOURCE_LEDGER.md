# ADA Command Center — Source Ledger

Estado: **AUDIT LEDGER — historia conservada; delta C1 estructural y delta C2 de configuración/identidad verificados en Git (2026-09-29)**. Los SHAs históricos describen únicamente sus cortes; **HEAD C2 actual** se registra separadamente.

## Autoridad actual para este reemplazo

```text
Implementación       moragaga/atlanticus:main@18029e19ff01e58b9c9399c132ff32b5ca913f06
Decisiones leídas    moragaga/atlanticus-decisions:main@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canónico previo      moragaga/atlanticus-cannonical:main@15ba51fb5a140601fd4e2a8d78a01a5b87c6eeaa
```

Esta actualización **no** cambia Git. Antes de integrar, comparar HEAD para evitar sobrescribir documentación posterior.

## Corte previo de Domain/Source v3 — HISTORICAL

```text
atlanticus antiguo checkpoint   880cb692054c2cd78cdc29cf62ab7b16bbd2c3d6
Domain Tools / Manifest         9b9600ae96c9153cf70d0fb401905963b8583c2f
Alarm Source Snapshot v3        d2a5e14822d3711e64668b8e70cfa15d7ddae2f0
canonical histórico            148b178df74ee3083681140f3bb7997a02435b80
```

`domain/tools` introdujo `ToolDependencyEntry`/`ToolDependencyManifest`. El snapshot actual mantiene `AlarmConfigurationSnapshot(configuration, tool_dependencies)`; `confirmed_tool_catalog_revision` se deriva del manifest. Source schema v2 SUPERSEDED, v3 CURRENT sin decoder v2. Workspace sidecar `_confirmed_tool_catalog_revision`, Validate/Publish drift guard y persistencia exacta de Tool dependencies permanecen. Pruebas históricas de aquel corte: domain/tools 8, domain/alarms 50, web/alarms/configuration 35, configuration-manager 11; no son métricas de C1 o C2.

## B1d histórico — qualification Tool local

```text
Implementación B1d         a518ff98c6303220e24ae3c645d3982e657fd22e
Inspección posterior       caced5d7711cf059d36ec61aecc9b3e9629bd41f
Decisions                 50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canónico del corte         ec16bd2ccf0ae06065b8ee1d3a231ef4d2cbac57
Canónico posterior B1d    a5bb42157ee7a5dd2fd64ccc43fa4519628ce25c
```

B1d calificó localmente dos Tool Sources/Projections Cosmos controladas, discovery/confirmación y Blob CURRENT en Azurite; Tool catalog revision observada `6a26feedc3cf7cee4ebcf5a93ad59314180635875ab25423bb576a052e517243`. El usuario informó 38 pruebas backend, 31 Manager y 6 de qualification, Ruff/format PASS. Una Alarm Source **local** observada no acreditó Alarm Source Blob/Projection Cosmos durable; `verify-alarm` no encontró esa pareja entonces. Los archivos de qualification temporal se limpiaron posteriormente. En B1d los services Tool estaban en `backend/tools`: descripción histórica SUPERSEDED por C1, nunca arquitectura vigente.

## C1 — Web Tool ownership CLOSED estructuralmente

```text
atlanticus C1           3961385aecd0eb7e373018fc25e509a71dccc409
commit previo           a4dc45fc7fa17ef20e6ddfa828bbb3a471c17f2d
commit ajeno intermedio d4239806c01f0f8bf4b4d3e680715ca460667bcb
canonical base C1       faec587c3fb321e76c1a3da38a4d8193a2f1fdb5
```

Verificado en Git y evidencias del usuario: `web/tools/catalog`, `web/tools/discovery-cosmos`, `web/tools/catalog-manager` son owners independientes; Manager temporal los compone sin UI duplicada. `backend/tools` desapareció como árbol versionado; se eliminaron nombres antiguos sin adapters. Se quitaron lock redundante individual de Materialization, se actualizaron dependencias/imports y se reportaron pruebas/Ruff/espejos/wheels/importaciones host favorables. **UNVERIFIED en C1:** distribución aislada, navegador final, CI, Azure y Starter.

## B2c.7 — historial operacional independiente

Referencia implementada histórica `atlanticus@c67fcb5b105cc561c16719a8bca4ea5aa74c3fae`: Runtime CURRENT v1 completo y FACTS v2 encadenados, receptor Delivery CURRENT+FACTS con cursor propio, integración local controlada/reinstanciación. La documentación previa registra múltiples gates B2c.7 locales, incluido un corte final con 32 pruebas específicas; estas cifras NO se reasignan a C1 ni a C2. Qualification Docker independiente nunca se infiere de ese corte.

## C2 — identidad, volumen y topología operacional (NUEVO CORTE, 2026-09-29)

```text
Base de implementación auditada antes de C2  53c20b2462e8637bd00f75487b0df05dc8452daa
HEAD final C2 leído en Git               18029e19ff01e58b9c9399c132ff32b5ca913f06
Decisions HEAD                            50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical HEAD aún anterior a C2          15ba51fb5a140601fd4e2a8d78a01a5b87c6eeaa
```

**DECIDED:** el usuario confirmó que no hay despliegues previos ni flujo productivo que migrar; se acordó limpieza sin backward compatibility. `APPLICATION=ada-command-center` común a tres jobs con distintos `job_key`; el operador conserva control manual sobre `VOLUMEN_PATH`, conexiones y evidence. Source Key literal `alarm-configuration` debe tener definición única Domain; Cosmos physical name y partition key derivan del contrato ya existente, no de `.env`; nombre Blob ambiental se conserva. C3/C4/C5 quedan explícitamente fuera de C2.

**VERIFIED Git:** `domain/alarms/identity.py` declara y exporta constante; Web la convierte a `SourceKey`; tres jobs derivan `settings.source_key`; `.env.detail`, `secrets.detail.json` y specs ya no contienen `ALARM_CONFIGURATION_SOURCE_KEY`; Materialization obtiene nombre y partición del resource contract Cosmos y quita `ALARM_PROJECTION_CONTAINER`. Tres plantillas/manifiestos usan `APPLICATION=ada-command-center`; los tres `job_key` independientes permanecen; Delivery conserva recepción CURRENT+FACTS. El commit final incorpora corrección del test `test_all_current_source_partitions_are_registered_without_future_sources` para fuentes que el registro Operational Data ya incluía. No se incluyen ni vuelven a editar tests en este paquete documental.

**VERIFIED por logs locales compartidos por el usuario durante C2:** Domain 56 PASS; backend 238 PASS, 1 SKIPPED, 1 DESELECTED al excluir la prueba previa al arreglo; Configuration Manager 28 PASS; Materialization C2 1 PASS; `uv lock --check` exitoso; integración ZIP/parche y `git diff --check` sin error en el momento reportado. La corrección del test de catálogo sí está en el commit final pero la ejecución integral posterior **no fue mostrada**.

**UNVERIFIED/OPEN:** backend completo luego del último commit, skipped gate y Ruff final; igualdad física cuenta/base Cosmos entre Web y Materialization, montajes reales separados, Docker multi-proceso, CI/Azure, instalación aislada, Starter y aceptación visual. Cierre C2 significa código/gates locales acotados, no Golden Path integral.

## Conflictos históricos no reescritos

Los Markdown `R3.6M-006B.2...` de Decisions siguen conteniendo formulaciones históricas que asignaban autoridad a SharePoint y describían reconciliación/salidas físicas distintas. Implementación/canónicos recientes usan Confirmed Tool Catalog en Blob y Alarm Source durable en Blob con Projection Cosmos de entrada y READY local; esa diferencia requiere reconciliación formal si se reabre el documento de decisions, no crear adaptadores ni devolver la implementación al diseño antiguo. Los documentos históricos también distinguen Live de Management; esa separación **se preserva**. No se realizó auditoría exhaustiva de todos los DOCX del repositorio Decisions.
