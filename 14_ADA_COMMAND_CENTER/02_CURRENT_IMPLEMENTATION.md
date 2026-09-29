# ADA Command Center — Current Implementation

Estado: **CURRENT — Source v3 / Materialization / Engine / Delivery input; C1 ownership Web CLOSED; C2 identidad y topología compartidas CLOSED estructuralmente con validación local acotada (2026-09-29)**. Ni C1 ni C2 certifican Docker/Azure o Golden Path productivo.

## Checkpoints

```text
atlanticus:main           18029e19ff01e58b9c9399c132ff32b5ca913f06  (C2 y corrección posterior del test operacional)
atlanticus-decisions:main 50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
canonical:main base      15ba51fb5a140601fd4e2a8d78a01a5b87c6eeaa
C1 histórico            3961385aecd0eb7e373018fc25e509a71dccc409
B2c.7 histórico         c67fcb5b105cc561c16719a8bca4ea5aa74c3fae
```

El commit actual fue leído directamente en Git; los resultados de las pruebas son evidencia proporcionada por el usuario y no ejecución remota independiente.

## Componentes existentes

```text
scopes/ada-command-center/
  domain/alarms/                             # AlarmConfigurationSnapshot v3 + identidad Source Key C2
  domain/tools/                              # ToolDependencyManifest compartido
  backend/alarms/core/                       # Engine puro
  backend/alarms/materialization/            # resolver B.2 y READY exacto
  backend/alarms/persistence/                # WAL, fencing y EFFECTIVE
  backend/alarms/contracts/                  # CURRENT v1 y FACTS v2
  backend/processes/alarms-materialization/  # adquiere Cosmos; qualification manual; publica READY/BLOCKED local
  backend/processes/alarms-runtime/          # consume READY/EFFECTIVE exacto; produce CURRENT y FACTS
  backend/processes/alarms-delivery/         # todavía recibe CURRENT+FACTS
  web/alarms/configuration/                  # authoring + integración Manager
  web/alarms/persistence/                    # composición Source/Projection
  web/alarms/projection-local/
  web/alarms/projection-cosmos/              # adaptador y resource contract Cosmos
  web/tools/catalog/                        # C1 Catalog y Blob
  web/tools/discovery-cosmos/                # C1 discovery nombrado
  web/tools/catalog-manager/                # C1 UI/callbacks propios
  web/application/ada-command-center-configuration-manager/ # host temporal
```

`backend/tools` permanece SUPERSEDED. Domain Tools es transversal; los services Python exclusivos de Web pertenecen a Web. Materialization aún importa `web/alarms/projection-cosmos`, frontera técnica OPEN que C2 NO refactorizó.

## C2 — identidad operacional y Source Key (CURRENT)

Los tres `.env.detail` y sus `secrets.detail.json` usan `APPLICATION=ada-command-center`. El valor es un contrato de configuración/despliegue; cada `JobDefinition` mantiene `service_name` y `job_key` únicos: `alarms-materialization`, `alarms-runtime` y `alarms-delivery`. Comparten el padre de runtime por APPLICATION sin compartir lease. `VOLUMEN_PATH` permanece variable **manual**, absoluta y referida al mismo almacenamiento físico/montaje en los tres procesos. Igualdad textual de la ruta no acredita por sí misma identidad física del volumen en diferentes contenedores.

`domain/alarms/identity.py` contiene una única constante de texto `ALARM_CONFIGURATION_SOURCE_KEY = 'alarm-configuration'`, exportada en la API Domain. El host Web crea su `SourceKey` técnico a partir de ese valor. Materialization, Runtime y Delivery exponen `settings.source_key` derivado de la constante; los tres eliminaron `ALARM_CONFIGURATION_SOURCE_KEY` de specs, `.env.detail` y secretos. La identidad guardada en artifacts, READY y EFFECTIVE sigue verificándose físicamente; no se sustituye por la constante sin validar.

Materialization deriva `projection_container` de `ALARM_CONFIGURATION_PROJECTION_STORAGE_RESOURCE.default_physical_name` y la partición de `resource.topology.partition_key_path`, en lugar de `ALARM_PROJECTION_CONTAINER` y `'/partition_key'` duplicados. El recurso declara el nombre `ada-command-center-alarm-configuration-projection`, partición `/partition_key` y solo permite override de referencia de conexión. `ALARM_COSMOS_ENDPOINT`, `ALARM_COSMOS_DATABASE_NAME` y su credencial siguen siendo ambientales/manuales. **Gate físico OPEN:** configurar Materialization contra la misma cuenta y base Cosmos que usa Web; compartir el contrato del contenedor no comprueba esa igualdad.

El nombre del contenedor Blob permanece configurable en Web. Las conexiones nombradas de Delivery (`config/connections.json`), sus destinos de publicación y `max_facts_per_iteration` permanecen CURRENT sin modificación funcional C2. No se dedujeron rutas del desarrollador a partir de APPLICATION, ni se introdujeron migraciones/compatibilidad: el usuario confirmó que no hay despliegue previo que preservar.

## Alarm pipeline previo conservado

1. `AlarmConfigurationSnapshot(configuration, tool_dependencies)` schema v3 congela referencias Rn/Cn; el Tool Catalog CURRENT está en Blob, no se reinterpreta desde latest.
2. Materialization obtiene `ProjectionRecord[AlarmConfigurationSnapshot]`, exige qualification externa actualmente desde JSON, resuelve B.2 y publica pareja READY `runtime.json`/`delivery.json` con manifest/hash; BLOCKED publica diagnóstico sin reemplazar READY.
3. Runtime selecciona pin exacto `source_key + result_id + manifest_sha256 + resolution_key`, adopta mediante WAL V1/V2 y proyecta EFFECTIVE; no consume simplemente el último READY.
4. Runtime emite `runtime/output/current/latest.json` CURRENT v1 completo/reemplazable tras durabilidad y exporta FACTS v2 inmutables encadenados a partir de commits durables.
5. **CURRENT pre-C4:** Delivery recibe CURRENT y FACTS v2 desde el volumen, valida pin/EFFECTIVE/READY exactos y mantiene cursor propio de recepción FACTS. Este receptor no es `AlarmLiveProjection` ni Analytics.

## Evidencia y límites C2

**VERIFIED por lectura de Git:** commit C2 actual, código/plantillas/manifiestos/lock, Source Key Domain y composición de resource contract; la prueba de registro operacional fue corregida en el commit actual por el usuario. **VERIFIED por logs locales del usuario antes de esa corrección:** Domain 56 PASS; backend 238 PASS, 1 SKIPPED y 1 test de catálogo excluido en el gate diagnóstico; host Configuration Manager 28 PASS; prueba específica Materialization 1 PASS; `uv lock --check` satisfactorio; `git diff --check` limpio durante integración local. La prueba original fallaba porque el registro Operational Data ya contenía `FABRICA_KPIS`, `METEODATA_DATA` y `METEODATA_PROJECTION`. El commit actual cambió las expectativas del test; no se editan ni incluyen tests backend nuevos con esta actualización documental.

**UNVERIFIED:** full backend suite sin exclusiones tras el commit actual; razón/alcance del SKIPPED; Ruff/format del commit actual; Docker procesos independientes, volúmenes multi-host, cuentas Cosmos/Blob reales, CI, distribución aislada y aceptación browser. No convertir gates parciales en certificación de infraestructura.

## OPEN separados

- C3 qualification real: productor/verificadores GREEN y evaluadores autorizados siguen sin acreditación.
- C4 Delivery CURRENT-only: todavía no implementado; FACTS v2 Runtime no debe eliminarse.
- C5 evidencia técnica en `.env`: `ALARM_TECHNICAL_EVIDENCE_CONTRACT_KEY/VERSION` aún requieren identificación contractual.
- Frontera Materialization → Web projection Cosmos pendiente de dueño técnico apropiado.
- Starter Web, Live, Management Capture, History/Analytics, UI fin de turno/modal, qualification Docker/Azure: alcances independientes.

No crear código, alias ni nuevas dependencias para solventar pendientes documentales durante el cierre de C2.
