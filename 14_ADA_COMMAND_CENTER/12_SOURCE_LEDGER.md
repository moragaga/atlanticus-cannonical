# ADA Command Center — Source Ledger

Estado: **AUDIT LEDGER — conservar historia B1d/B2c.7/C1/C2; sumar checkpoint C4 CURRENT-only confirmado por lectura de Git y logs locales del usuario (2026-09-29).** Los SHAs históricos no se redefinen como HEAD actual.

## Autoridad de este reemplazo

```text
Implementación actual  moragaga/atlanticus:main@45eff96d777f4711cb011f779ffc0a6c87bf0ca4
Implementación C2      moragaga/atlanticus@18029e19ff01e58b9c9399c132ff32b5ca913f06
Decisions              moragaga/atlanticus-decisions:main@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical base C4      moragaga/atlanticus-cannonical:main@2e8bbf4780cafc4cea3b18351861aa97a4fb0053
```

Esta actualización documental no escribe Git. El HEAD canonical base **ya incluye las nueve sustituciones C2**; no reutilizar el paquete que tenía base canonical `15ba51...`.

## Corte previo Domain / Source v3 — HISTORICAL

```text
atlanticus antiguo checkpoint  880cb692054c2cd78cdc29cf62ab7b16bbd2c3d6
Domain Tools / Manifest        9b9600ae96c9153cf70d0fb401905963b8583c2f
Alarm Source Snapshot v3       d2a5e14822d3711e64668b8e70cfa15d7ddae2f0
canonical histórico           148b178df74ee3083681140f3bb7997a02435b80
```

`domain/tools` introdujo `ToolDependencyEntry` y `ToolDependencyManifest`. `AlarmConfigurationSnapshot(configuration, tool_dependencies)` conserva Cn congelado derivado de manifest. Source v2 SUPERSEDED, v3 CURRENT, sin decodificador v2. El workspace mantiene `_confirmed_tool_catalog_revision`, Validate/Publish drift guard y dependencies exactas. Evidencia local de ese corte: Domain Tools 8, Domain Alarms 50, Alarm Web 35, Configuration Manager 11; no son conteos C1/C2/C4.

## B1d — qualification Tool local HISTORICAL

```text
Implementación B1d        a518ff98c6303220e24ae3c645d3982e657fd22e
Inspección posterior      caced5d7711cf059d36ec61aecc9b3e9629bd41f
Decisions                50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical del corte       ec16bd2ccf0ae06065b8ee1d3a231ef4d2cbac57
Canonical posterior B1d  a5bb42157ee7a5dd2fd64ccc43fa4519628ce25c
```

B1d calificó localmente dos Tool Sources/Projections Cosmos controladas, discovery/confirmación y Blob CURRENT en Azurite; Tool catalog revision observada `6a26feedc3cf7cee4ebcf5a93ad59314180635875ab25423bb576a052e517243`. El usuario informó 38 pruebas backend, 31 Manager y 6 qualification, Ruff/format PASS. Una Alarm Source local observada **no** acreditó Alarm Source Blob/Projection Cosmos durable; `verify-alarm` no encontró la pareja física entonces. Archivos de qualification temporal fueron limpiados. El owner histórico `backend/tools` de B1d fue SUPERSEDED en C1.

## C1 — ownership Tool Web CLOSED

```text
atlanticus C1            3961385aecd0eb7e373018fc25e509a71dccc409
commit previo            a4dc45fc7fa17ef20e6ddfa828bbb3a471c17f2d
commit ajeno intermedio  d4239806c01f0f8bf4b4d3e680715ca460667bcb
canonical base C1        faec587c3fb321e76c1a3da38a4d8193a2f1fdb5
```

`web/tools/catalog`, `web/tools/discovery-cosmos` y `web/tools/catalog-manager` se hicieron owners de servicios Web, UI/callbacks y composición del host temporal; `backend/tools` dejó de ser tree versionado sin adaptadores. Se reportaron tests/Ruff/espejos/wheels/importaciones host satisfactorias. Distribución aislada, browser final, CI, Azure y Starter siguieron UNVERIFIED.

## B2c.7 — historial operacional independiente

En `atlanticus@c67fcb5b105cc561c16719a8bca4ea5aa74c3fae` Runtime ya producía CURRENT v1 completo y FACTS v2 durables encadenados y el **receptor de entonces** recibía ambos con cursor propio. B2c.7 registró pruebas locales controladas y reinstanciación, incluido un corte de 32 pruebas específicas. La descripción CURRENT+FACTS es **HISTORICAL/SUPERSEDED por C4 únicamente para el receptor Delivery**. No se alteró productor FACTS v2 de Runtime ni se acreditó qualification Docker.

## C2 — identidad, volumen y topología (HISTORICAL / CURRENT CONTRACT)

```text
Implementación base antes C2  53c20b2462e8637bd00f75487b0df05dc8452daa
HEAD final C2              18029e19ff01e58b9c9399c132ff32b5ca913f06
Decisions C2               50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical C2 integrado     2e8bbf4780cafc4cea3b18351861aa97a4fb0053
```

**DECIDED C2:** no existen despliegues previos que conservar; los tres jobs usan `APPLICATION=ada-command-center` pero conservan `job_key`/leases independientes. `VOLUMEN_PATH` absoluta y físicamente compartida es administrada por operador; source key `alarm-configuration` se define en Domain. Cosmos physical name/partition derivan del resource contract, no de `.env`; Blob container sigue ambiental.

**VERIFIED Git C2:** Domain exporta constante; Web construye `SourceKey`; tres procesos derivan `settings.source_key` sin env/secret duplicado. Materialization consume el resource contract y eliminó `ALARM_PROJECTION_CONTAINER`; tres plantillas usan APPLICATION común, leases separados. Commit incluyó corrección del test global Operational Data `test_all_current_source_partitions_are_registered_without_future_sources`; no reintroducir tests previos ni adaptadores.

**Evidencia C2 histórica del usuario:** Domain 56 PASS; backend 238 PASS, 1 SKIPPED y 1 DESELECTED antes del arreglo; Configuration Manager 28 PASS; test específico Materialization 1 PASS; `uv lock --check` PASS. No atribuirle la regresión nueva C4. Igualdad física Cosmos, volumen multi-proceso y Docker seguían OPEN.

## C4 — receptor Delivery CURRENT-only (NUEVO CORTE, 2026-09-29)

```text
Base C4       18029e19ff01e58b9c9399c132ff32b5ca913f06
HEAD C4       45eff96d777f4711cb011f779ffc0a6c87bf0ca4
Decisions     50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical     2e8bbf4780cafc4cea3b18351861aa97a4fb0053  (sin C4 al auditar)
```

**DECIDED por el usuario en este hito:** Delivery debe consumir solo el último CURRENT, esperar sin incorporar cuando CURRENT/EFFECTIVE/READY exacto no coincidan y reanudar en un ciclo posterior cuando coincidan. No introducir sincronización extra entre Materialization, Runtime y Delivery. Runtime mantiene FACTS v2/WAL independientes. El usuario describió su cadencia operacional prevista Materialization 5 minutos, Runtime y Delivery 10 minutos; no se observó un despliegue que pruebe esas frecuencias y Delivery `.env.detail` de código conserva `ALARM_DELIVERY_POLL_SECONDS=5` como valor predeterminado: diferenciar diseño operativo y configuración física.

**VERIFIED Git:** comparación base→HEAD en exactamente un commit con 14 archivos modificados todos bajo `backend/processes/alarms-delivery`. `receiver.py` quitó recepción/cursor FACTS y consume latest CURRENT con integridad, EFFECTIVE/pin/READY, tiempo UTC y fence. `job.py` elimina facts de resultado/iteration facts; `settings.py`, `.env.detail` y `secrets.detail.json` quitan `ALARM_DELIVERY_MAX_FACTS_PER_ITERATION`. Se actualizaron tests y espejos. `bootstrap.py` mantiene registry Cosmos/publisher paralelo, solo elimina el argumento FACTS al construir job. Ningún archivo Runtime fue tocado.

**VERIFIED por logs locales del usuario:** 29 PASS Delivery, 16 PASS publicadores Runtime, Ruff de código/tests C4 PASS, `git diff --check` y `uv lock --check` PASS. La primera suite global falló en colección por paquete workspace Materialization retirado por `uv sync --locked` normal; se corrigió el entorno con `uv sync --locked --all-packages`, import PASS y regresión global **567 PASS / 1 SKIPPED** con `uv run --locked --all-packages pytest` en Python **3.14.2**. No atribuir la colección fallida a regresión del código C4. Error Ruff SIM117 preexistente en `test_parallel.py` fuera de los 14 archivos.

**UNVERIFIED:** Docker separado, CI, Azure, distribución wheel aislada, montaje real compartido, comportamiento temporal de planificador en producción y despacho Live. No declarar Live implementado porque exista inbox CURRENT.

## Conflictos históricos no reescritos

Los documentos B.1/B.2 antiguos en Decisions conservan formulaciones históricas sobre SharePoint y reconciliación/salidas físicas diferentes: code/canonical actuales establecen Blob para Tool Catalog y objetivo durable Alarm Source con Cosmos Projection de entrada, READY local y Runtime WAL/EFFECTIVE. La separación Live/Management histórica permanece compatible. No se auditaron exhaustivamente todos los DOCX de Decisions. Special Cascade, Messages inactive y límites adoption siguen abiertos donde están documentados; no resolverlos por C4. Ninguna decisión vigente de Decisions se reescribió en Git en este hito.
