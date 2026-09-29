# ADA Command Center — Current Implementation

Estado: **CURRENT — B2c.7 Engine/Delivery histórico (2026-09-28), B1d Tool qualification histórica (2026-09-29) y C1 Web Tool Ownership CLOSED estructuralmente (2026-09-29)**. No transferir una qualification de un frente a otro. Implementación C1 verificada en `moragaga/atlanticus:main@3961385aecd0eb7e373018fc25e509a71dccc409`; Decisions HEAD `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`; Canonical base previa `faec587c3fb321e76c1a3da38a4d8193a2f1fdb5`. Las comprobaciones locales de C1 no equivalen a CI productiva.

## Componentes relevantes — árbol actual

```text
scopes/ada-command-center/
  domain/alarms/                             # Alarm authored configuration v3/routing
  domain/tools/                              # ToolDependencyManifest/Entry compartidos
  backend/alarms/core/                       # Engine core
  backend/alarms/materialization/            # B.2, resolver, lector READY exacto
  backend/alarms/persistence/                # WAL/recovery/EFFECTIVE
  backend/alarms/contracts/                  # CURRENT v1 y FACTS schemas
  backend/processes/alarms-materialization/  # job READY/BLOCKED local
  backend/processes/alarms-runtime/          # sesión, CURRENT y FACTS
  backend/processes/alarms-delivery/         # input receiver y cursor propio actual
  web/alarms/configuration/                  # authoring y manager capability
  web/alarms/projection-local/               # projection local
  web/alarms/projection-cosmos/              # projection Cosmos de entrada
  web/alarms/persistence/                    # Source/Projection composition
  web/tools/catalog/                        # consolidación y Blob Catalog CURRENT
  web/tools/discovery-cosmos/                # discovery por conexiones nombradas
  web/tools/catalog-manager/                # UI + callbacks reutilizables
  web/application/ada-command-center-configuration-manager/ # host temporal
```

`backend/tools` dejó de ser directorio versionado. C1 no crea Starter ni convierte web/alarms en motor de evaluación. El backend Materialization todavía consume un adapter de proyección situado en Web: frontera estructural diferida, no resuelta incidentalmente.

## Estado auditable de alarmas — conservado desde B2c.7

| Elemento | Estado |
|---|---|
| Domain/Core, Source v3 y Tool manifest exacto | CURRENT preexistente. |
| Tool Cn, Validate/Publish drift guard, codec y adapters Local/Cosmos | CURRENT preexistente; Azure físico UNVERIFIED. |
| Strict routing y resolver B.2 puro | CURRENT; I/O separado donde ya existe. |
| Materialization READY/BLOCKED local, pareja Runtime/Delivery con manifest/hashes | CURRENT; salida B.2 Cosmos monolítica antigua SUPERSEDED. |
| B1 pin exacto y planificación de adopción | CURRENT. |
| B2a adoption WAL V1 (0 grupos) / V2 (1..N) | Ambos CURRENT según sus contratos, no aliases legacy. |
| B2b Effective Head desde WAL y lector Runtime exacto | CURRENT. |
| `alarms-runtime` con bootstrap/registry/source adapter inyectados y sesión fijada | CURRENT; fuentes físicas/productivas UNVERIFIED. |
| Engine CURRENT v1 y FACTS v2 encadenados | CURRENT, gates locales B2c.7 reportados; no asumir Azure. |
| `alarms-delivery` receptor CURRENT+FACTS y cursor propio | CURRENT todavía en código; reemplazo de consumo FACTS por latest CURRENT es C4 PLANNED. |
| Integración Engine→Delivery con datos controlados y reinstanciación | CLOSED gate local histórico; Docker procesos separados UNVERIFIED. |
| Migración desde volúmenes FACTS v1 | OPEN si se encuentra estado afectado; sin adapter legacy. |
| Live materializer/AlarmLiveProjection, Management Capture y History/Analytics | PLANNED/SEPARATE; no inferir de input receiver. |

## Tool Catalog Web — B1d previo y estado C1 actual

**B1d histórico:** `backend/tools/catalog` y `backend/tools/discovery-cosmos` existían; Tool Catalog UI estuvo acoplado a `web/application/ada-command-center-configuration-manager` mediante `catalog_manager.py`. Esta descripción histórica es **SUPERSEDED** por el commit C1 y no debe usarse para implementar nuevos consumidores.

**CURRENT C1:** `web/tools/catalog` y `web/tools/discovery-cosmos` son los propietarios de snapshot/consolidación/Blob y discovery/confirmación respectivamente; `web/tools/catalog-manager` ya posee UI/callbacks y expone factory Manager reutilizable. El host temporal compone estas tres bibliotecas e integra `ManagerEntry('/tool-catalog')` dentro de `/manager`; NO se convierte en página Dash autónoma ni en Starter distribuible.

`web/alarms/configuration` sigue siendo capability independiente. El host posee `__main__` local, proveedor `local|durable` y lectura de `.env` propia: en local Alarm Source/Projection pueden residir en filesystem mientras Tool Catalog usa Blob; durable utiliza Alarm Source Blob y Projection Cosmos. El nombre de contenedor Blob del Manager **sí** permanece en `.env`; los nombres físicos de contenedor Cosmos se obtienen de resource contracts. Las capacidades existentes no prueban que el despliegue durable de Alarm Source/Projection funcione físicamente E2E.

**VERIFIED C1 en Git:** el commit `3961385a...` trasladó ambos servicios completos, tests y espejos a Web; actualizó consumidores/lockfiles y quitó `backend/processes/alarms-materialization/uv.lock` redundante del workspace. Los imports públicos antiguos ya no son parte de la distribución. El usuario informó pruebas de cada paquete afectado y suite conjunta Materialization/Runtime GREEN, Ruff/format GREEN y builds de ambos wheels nuevos; un smoke de importación de los tres paquetes en el venv del host pasó. No hay archivos Git bajo `backend/tools`.

**HISTORICAL B1d local:** dos Tool Sources/Projections de prueba en bases Cosmos independientes, discovery READY, confirmación y `verify-catalog` en Azurite; 38 backend tests, 31 Manager y 6 qualification de ese corte. El usuario observó Tools en el Manager y una Source Alarm local. No declarar fuentes/proyecciones Alarm durables ni CI/Azure a partir de esos logs. Los scripts temporales B1d fueron limpiados en el hito posterior de cleanup, no trasladados al producto.

## Materialization — contrato vigente y próximos deltas

La afirmación canónica histórica «B.2 sólo publica Cosmos y salida local vendrá después» es **SUPERSEDED**. Materialization publica localmente versiones READY inmutables con manifest y `runtime.json`/`delivery.json` o BLOCKED diagnóstico, sin dual-output Cosmos antiguo. Proyección Cosmos sigue siendo posible entrada; Tool Catalog NO se reconsulta desde latest para reinterpretar un snapshot Rn/Cn ya congelado.

Qualification actual requiere JSON externo (`ALARM_QUALIFICATIONS_FILE`) y no tiene productor productivo verificado: su automatización es C3 PLANNED, no C1/C2. Registro de evaluadores productivo vacío; ejemplo `mina.threshold` no es implementación productiva. Las tres plantillas `.env.detail` aún tienen APPLICATION distintas y Source Key repetida, Materialization conserva contenedor Cosmos configurable y Runtime pareja de evidencia técnica ambiental: **C2/C5 pendientes**. Delivery sigue consumiendo FACTS además de CURRENT: **C4 pendiente**.

## Evidencia / no objetivos / siguiente frente

B2c.7 históricamente informó 137/151/152/162 PASS en hitos sucesivos más un SKIPPED de gate de wheels, 32 PASS específicas en el último corte; Ruff GREEN. C1 presentó gates locales por paquetes afectados y construcción de dos wheels, **sin** instalación aislada ni browser post-C1. No hay certificación física nueva de Cosmos/Blob productivos, PI datasets reales, Azure, Docker independiente ni multi-host por C1.

**Siguiente foco único C2:** evaluar contratos actuales y normalizar APPLICATION/rutas/Source Key/contendor Cosmos de los tres jobs, preservando Blob container en `.env`, exact pin y no legacy. No mezclar en C2 Qualification producer, cambio de Delivery, Live/History, Starter ni mejoras visuales.
