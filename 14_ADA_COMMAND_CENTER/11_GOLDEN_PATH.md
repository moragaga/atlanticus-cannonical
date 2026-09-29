# ADA Command Center — Golden Path

Estado: **PARTIALLY IMPLEMENTED — C1 CLOSED; C2 identidad/configuración contractual CLOSED con gates locales acotados; B2c.7 Engine/Delivery integrado históricamente en local; Source/Projection durable E2E, qualification C3, Delivery C4, Live/Web/History y Docker/Azure UNVERIFIED/PLANNED**. Checkpoint C2 remoto: `atlanticus:main@18029e19ff01e58b9c9399c132ff32b5ca913f06`.

## Recorrido y evidencia

| Etapa | Owner | Estado acotado |
|---|---|---|
| Tool Sources/Projections y discovery por conexiones nombradas | Tool/Web | CURRENT; B1d qualification local histórica, no Azure productivo. |
| Confirmed Tool Catalog Cn → Blob CURRENT | `web/tools/catalog` | CURRENT; C1 Web ownership CLOSED. |
| Discovery/inspect/confirm | `web/tools/discovery-cosmos` | CURRENT; C1 CLOSED. |
| Tool Catalog UI capability reusable | `web/tools/catalog-manager` | CURRENT; C1 CLOSED; browser final UNVERIFIED. |
| Alarm authoring Rn/Cn: Save/Validate/Publish + drift guard | Alarm Web/Domain | CURRENT Source v3. |
| Source Key única `alarm-configuration` | `domain/alarms` + Web y jobs consumidores | CURRENT; C2 CLOSED. |
| Source Blob / Alarm Projection Cosmos de entrada | Web/Materialization | Adapters y contrato físico CURRENT; conexión E2E durable UNVERIFIED. |
| Cosmos physical name y partición derivadas de resource contract | Web + Materialization | CURRENT; C2 CLOSED, sin `ALARM_PROJECTION_CONTAINER` ambiental. |
| Tres jobs APPLICATION común, leases separados y volumen administrado | Procesos Alarm | CURRENT C2; montaje real multi-proceso UNVERIFIED. |
| Qualification de candidato Rn/Cn | Materialization | CURRENT JSON de entrada manual; C3 productor automático BLOCKED BY DESIGN. |
| B.2 READY/BLOCKED y pareja de artefactos exactos | Materialization | CURRENT, validado en tests locales anteriores. |
| WAL adopción → EFFECTIVE exacto | Persistence/Runtime | CURRENT, gate histórico local. |
| Engine CURRENT v1 completo | Runtime | CURRENT; gate histórico local. |
| Engine FACTS v2 encadenados a partir de commits durables | Runtime | CURRENT; **preservar al ejecutar C4**. |
| Receptor independiente CURRENT **y FACTS** con cursor propio | Delivery input | CURRENT; su reducción a CURRENT-only es **C4 PLANNED**. |
| Motor Engine→Delivery bajo filesystem controlado/reinicio | Gate local previo | CLOSED histórico; Docker/multi-host UNVERIFIED. |
| Contrato técnico evidencia Runtime e inspección restante de env | C5 | PLANNED; propietario/key/version OPEN. |
| Starter Command Center Web y aceptación durable UI | Web | PLANNED / UNVERIFIED. |
| Live materializer + AlarmLiveProjection | Live | CONTRACT AGREED / NOT IMPLEMENTED. |
| Management Capture/Projection, History/Analytics | Frentes independientes | PLANNED. |

## Flujo operacional y contratos congelados

```text
Tool Sources/Projections -> Confirmed Tool Catalog Cn (Blob)
      -> Alarm Configuration Web: Source Rn + manifest Cn (v3)
      -> Alarm Projection Cosmos (contrato físico compartido C2)
      -> Materialization con qualification manual actual
           +-- BLOCKED: diagnóstico; no promover artifacts
           `-- READY: manifest exacto + runtime.json + delivery.json
      -> Engine WAL adoption/rehydration -> EFFECTIVE exacto
      -> Engine CURRENT v1 (snapshot reemplazable)
      -> Engine FACTS v2 (hechos durables, flujo propio)
      -> Delivery input CURRENT+FACTS (CURRENT; reemplazo C4 PLANNED)
      -> Live / Web / History: NO implementados por este gate
```

El mismo `AlarmResolutionKey` y el mismo pin completo `source_key + result_id + manifest_sha256 + resolution_key` rigen consumidores. `INVALID != REMOVED`, `DISABLED != REMOVED`, `TRACE_ONLY != REMOVED`; READY no activa Engine. Runtime y Delivery no deben reobtener configuración de Cosmos ni sustituir una revisión exacta con latest READY.

C2 reduce inconsistencia de configuración, no elimina decisiones manuales del operador: `VOLUMEN_PATH` físicamente compartida, binding Cosmos a misma cuenta/base, rutas PI/productores y evidencia qualification requieren entrada explícita. No hay migración de despliegues: el usuario confirmó que no se había desplegado anteriormente. El Blob container sigue ambiental.

## Evidencia de cierre C2 y gate faltante

El usuario informó `domain/alarms` 56 PASS, backend 238 PASS/1 SKIPPED/1 DESELECTED en el gate diagnóstico previo a corrección, Configuration Manager 28 PASS, un test específico Materialization 1 PASS, `uv lock --check` satisfactorio. El commit C2 final incluye la corrección de la prueba operacional, verificada en Git. **UNVERIFIED:** ejecución sin `-k` después del commit final, causa del SKIPPED, instalaciones aisladas, hardware/servicios externos, CI, Docker y navegador. Las cifras no se mezclan con B1d/C1/B2c históricos.

## Frontera propuesta después de C2

**C4 Delivery CURRENT-only**, en otro chat y con etapa previa de diseño. No mezclar productor C3, contrato de evidencia C5, LIVE, Analytics, Tool Catalog, UX o Docker. La qualification Docker de Engine/Delivery continúa como gate distinto, no se declara resuelta por C2.
