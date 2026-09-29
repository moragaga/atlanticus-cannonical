# ADA Command Center — Current Implementation

Estado: **CURRENT — B2c.7d Engine/Delivery (corte 2026-09-28) y B1d Web Tool Catalog (delta 2026-09-29), auditados con evidencias separadas**. Engine: `atlanticus@c67fcb5b105cc561c16719a8bca4ea5aa74c3fae`. B1d: `a518ff98c6303220e24ae3c645d3982e657fd22e`, presente en `main@caced5d7711cf059d36ec61aecc9b3e9629bd41f`. Decisions `50c2bb3...`. Los logs locales no equivalen a CI limpia.

## Componentes relevantes

```text
scopes/ada-command-center/
  domain/alarms/                            # Alarm Configuration v3, routing
  domain/tools/                             # ToolDependencyManifest
  backend/alarms/core/                      # Engine domain operations
  backend/alarms/materialization/           # resolver B.2 y lector exacto local
  backend/alarms/persistence/               # WAL/recovery/EFFECTIVE
  backend/alarms/contracts/                 # CURRENT v1, FACTS v1 historial + v2 runtime
  backend/processes/alarms-materialization/ # READY/BLOCKED local
  backend/processes/alarms-runtime/         # sesión, ejecutable, CURRENT y FACTS
  backend/processes/alarms-delivery/        # receptor independiente y cursor propio
  web/alarms/configuration/                 # authoring; sin cambios en B2c.7
  web/alarms/projection-local/              # contratos anteriores; no revalidados
  web/alarms/projection-cosmos/             # entrada/proyección según dominio; no salida B.2
  web/alarms/persistence/                   # editor/persistencia; no revalidado
```

## Estado auditable para alarmas

| Elemento | Estado |
|---|---|
| Domain/Core, Source v3 y Tool manifest exacto | CURRENT preexistente. |
| Tool Cn, Validate/Publish drift guard, codec y adapters Local/Cosmos | CURRENT preexistente; Azure físico UNVERIFIED. |
| Strict routing y pure B.2 resolver | CURRENT; paquete B.2 incluye I/O separado. |
| Materialization READY/BLOCKED local, parejas Runtime/Delivery con manifest/hash | CURRENT; salida B.2 Cosmos antigua SUPERSEDED. |
| B1 pin exacto y planificación de adopción | CURRENT. |
| B2a adoption global WAL V1 (0 grupos) / V2 (1..N) | Ambos CURRENT, no legacy. |
| B2b Effective Head desde WAL + lector Runtime exacto | CURRENT. |
| B2c `alarms-runtime` con bootstrap, registry/source adapter inyectados y sesión fijada | CURRENT; fuentes físicas UNVERIFIED. |
| B2c.7a Engine CURRENT v1 y FACTS, refinado a FACTS v2 en d | CURRENT, gates locales reportados. |
| B2c.7b job `alarms-delivery` de recepción separada y cursor propio | CURRENT, gates locales reportados. |
| B2c.7c integración Engine real/Delivery, datos controlados y restart de instancias | CLOSED gate local; Docker independiente UNVERIFIED. |
| B2c.7d FACTS v2 encadenado y recovery estricto | CLOSED gate local; volúmenes v1 existentes BLOCKED sin decisión. |
| Live materializer/`AlarmLiveProjection`, Management Capture, History/Analytics | PLANNED/SEPARATE; NO inferir de `alarms-delivery` input. |

## B1d — inventario Web/Tool Catalog incorporado (sin recalificar Engine)

**CURRENT en main verificado:** `backend/tools/catalog` mantiene `ToolCatalogSnapshot` y Storage; `backend/tools/discovery-cosmos` mantiene inspección/confirmación de varias conexiones y los controles de drift. La UI y callbacks del catálogo **todavía residen** en `web/application/ada-command-center-configuration-manager/.../catalog_manager.py`, integrados mediante `ManagerEntry(route='/tool-catalog')` y prefijo `/manager` de la aplicación. `pages/manager.py` sólo registra `/manager`: la UI Tool no es una página independiente. Alarm Configuration ya reside en su propia capability `web/alarms/configuration`.

El host actual tiene `__main__` para entorno `local`, proveedor `local|durable`, lectura de `.env` desde el directorio del host, Blob compartido para catálogo y distintas conexiones Cosmos por nombre. Con proveedor `local`, Alarm Source y Projection utilizan filesystem; con `durable`, Alarm Source usa Blob y Projection Cosmos. La existencia de estos adapters en código no certifica el flujo físico durable de alarmas.

**VERIFIED por logs del usuario:** backend 38 tests; Manager Web 31; qualification 6; dos Tool Sources/proyecciones reales de prueba en bases Cosmos separadas, discovery READY y catálogo confirmado/contrastado en Azurite. El usuario observó Tools en el Manager y persistió una Alarm Source local. **UNVERIFIED:** Source y Projection Alarm en modo durable, CI limpia, distribución de Starter y aceptación visual final de Tool Catalog. El verificador durable devolvió ausencia de Source/Projection.

**DECIDED objetivo / PLANNED implementación:** extraer la UI del catálogo a una librería `web/tools/...` independiente del host, hacer que el host la consuma y componer un Starter distribuible propio de Command Center. El directorio `qualification/tool-catalog-b1d` consta en trabajo local, **no** en el árbol remoto inspeccionado; inventariar antes de eliminar archivos o retenerlos como tests productivos.

## Materialization: estado anterior reemplazado

La afirmación del canonical histórico «Materialization sólo publica documento Cosmos y su salida local es el siguiente incremento» está **SUPERSEDED**. Materialization escribe versiones READY locales inmutables y BLOCKED diagnóstico con hashes, validación de lector y sin salida Cosmos dual. La proyección Cosmos de **entrada** puede seguir participando; Tool catalog confirmado en su fuente durable objetivo no se reconstruye desde latest al consumir una release Alarm Rn/Cn.

El JSON manual controlado de qualification no acredita productor Tool GREEN/Evaluator real. Ejemplo `mina.threshold` bajo `catalog/examples` no se registra automáticamente y el catálogo productivo permanece vacío hasta autorización expresa.

## Evidencia y no objetivos

B2c.7a 137 PASS/1 SKIPPED; b 151 PASS/1 SKIPPED; c 1 PASS integración y 152 PASS/1 SKIPPED; d 32 PASS específicas y 162 PASS/1 SKIPPED; Ruff lint/format PASS según logs del usuario. El commit del hito `c67fcb5...` ya fue inspeccionado en Git; no se reejecutó CI/checkout limpio de ese SHA. Sin Cosmos/Blob físicos, PI datasets reales, Docker de ambos jobs, volumen multi-host ni migración de estado FACTS v1 validada.

**Siguiente foco único:** revisar builds/distribución y ejecutar procesos independientes Engine/Delivery en Docker. No modificar Web Editor, Live/History ni contratos de negocio incidentalmente.
