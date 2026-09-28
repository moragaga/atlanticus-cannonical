# Web Platform — Resource Provisioning

Estado: **CURRENT / ADA GENERIC RESOURCE PREPARATION 001 CLOSED FOR LOCAL SCOPE / GLOBAL INVENTORY OPEN**  
Implementación actual leída: `moragaga/atlanticus:main@da75752e87036b8318f38f8d405c55e8cb18717d` (2026-09-28).  
Decisiones consultadas: `moragaga/atlanticus-decisions:main@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`.  
Qualification: pruebas y salidas Docker comunicadas por el usuario; no equivalen a CI ni a ejecución de Azure sobre este commit.

## Principio vigente

La preparación de recursos de aplicación pertenece al ámbito de Web/despliegue. Los jobs Backend no deben crear ni verificar toda la infraestructura en cada ejecución. El plan global `ApplicationResourcePlan` permanece **PLANNED / OPEN** hasta trazar los componentes de cada aplicación y sus requirements reales:

```text
Tool/Application
→ Components
→ capabilities/runtimes
→ persistence/projections
→ resource requirements
→ container inventory
```

No extrapolar el plan parcial de ADA Generic Manager a Command Center, KPI Delivery, alarmas, User Activity ni otras aplicaciones. Evitar contenedores innecesarios y autoridades duplicadas.

## Capacidades reutilizadas — CURRENT

```text
WEB-STORAGE-TOPOLOGY             CLOSED / CURRENT
USERS-STORAGE-TOPOLOGY           CLOSED / CURRENT
STORAGE-PREFLIGHT-COSMOS-BRIDGE  CLOSED / CURRENT
```

El bridge transforma `ResolvedStoragePlan` en `CosmosContainerSpec`, agrupa por `connection_ref` y utiliza `ensure_containers` o `validate_containers`. No crea bases de datos, no recibe secretos ni posee readiness de aplicación. `CosmosProvisioner` prepara explícitamente la base local cuando corresponde. Las conexiones múltiples y nombradas siguen siendo una capacidad del núcleo genérico Atlanticus; la conexión única de ADA Manager en esta etapa no limita ese contrato.

## ADA Generic Manager — plan parcial CURRENT

Se usan una conexión/base Cosmos y un contenedor Blob compartido. Los seis recursos Cosmos son:

| Logical ID | Contenedor físico | Partition key |
|---|---|---|
| `navigation.projection` | `navigation-projection` | `/partition_key` |
| `users.support` | `users-support` | `/partition_key` |
| `users.runtime` | `users-runtime` | `/id` |
| `ada.tools.projection` | `ada-tool-projection` | `/partition_key` |
| `ada.kpis.registry.projection` | `ada-kpi-registry-projection` | `/partition_key` |
| `ada.kpis.definition.projection` | `ada-kpi-definition-projection` | `/partition_key` |

El plan declara `default_ttl_seconds=None`. Profiles y ADA Access usan `users-support`; no forman contenedores adicionales. `users-runtime` no se fusiona con `users-support`. Las especificaciones físicas son contratos de capabilities y no nuevas variables `.env` para nombres de contenedores Cosmos.

Composición vigente:

```text
scopes/ada/web/application/ada-generic-application/src/ada/web/application/generic/
  manager_persistence.py
  manager_deployment.py
  resource_preparation.py
```

## Blob y namespaces

El contenedor compartido se especifica mediante `ADA_TOOL_SOURCE_BLOB_CONTAINER_NAME`. La connection string identifica la cuenta; también existe el modo SAS conforme a `StorageSettings`. Un contenedor físico no reemplaza la distinción lógica:

```text
<application_namespace>/sources/...
<application_namespace>/users/users.json.gz
<application_namespace>/<tool_namespace>/sources/...
```

`BlobSourceStore` agrega el tramo `sources/<SourceKey codificado>/...`: no duplicarlo en la composición. El namespace Users es de aplicación, no uno nuevo por herramienta.

## Incremento 001: preparación explícita — CURRENT

Entrypoint existente:

```text
ada-generic-manager-resources prepare
ada-generic-manager-resources validate
```

El comando anterior `ensure-local` **fue sustituido** por `prepare` para admitir la distinción de entorno. No introducir otro alias legacy.

`prepare_manager_resources` emite un informe estructurado por recurso: `CREATED`, `READY`, `MISSING`, `INCOMPATIBLE`, `FAILED`, `BLOCKED` y `SKIPPED`, con resultado global `COMPLETED`, `PARTIAL` o `FAILED`. Los errores parciales no impiden intentar los recursos independientes; los dependientes de una base inaccesible quedan `BLOCKED`. El observer de errores es opcional y aislado del flujo.

**Local:** `prepare` crea el contenedor Blob si falta, crea la base Cosmos si falta y asegura los seis contenedores. `validate` verifica recursos sin crearlos. Se rechazan topologías incompatibles; nunca corregir partición/TTL destruyendo recursos existentes.

**Production:** la cuenta y la base Cosmos se consideran preexistentes; `prepare` valida el acceso a la base y asegura únicamente contenedores Cosmos faltantes. El Blob queda `SKIPPED`, tanto en `prepare` como en `validate`: **ninguna operación ni health check Blob en preparación productiva**. No afirmar que el alcance Cloud fue ensayado con Azure.

La configuración inválida, las conexiones ausentes y el fallo anterior a la preparación son errores de entrada/runtime, distintos de un resultado parcial de recursos.

## Qualification local del incremento 001 — CLOSED PARA EL ALCANCE ENSAYADO

**VERIFIED STATIC:** el commit `da75752` incluye la implementación, espejos pedagógicos, pruebas y `full.yaml` actualizado. `web` depende del inicio de los dos emuladores, **no** del éxito del job `resources`; no reinterpretar esto como disponibilidad funcional universal del Manager.

**VERIFIED USER-REPORTED:** pytest del proyecto ADA Generic y tooling de distribución terminó completo sin fallos (~295 pruebas por los contadores reportados); Ruff de los archivos del incremento y `git diff --check` pasaron antes del commit. Distribución nueva: 67 wheels internos, build de imagen `ada-generic:resource-validation-001`, `PRECHECK_PASS`, instancias locales de Cosmos Emulator y Azurite. No se volvió a ejecutar aquí la suite contra un checkout limpio de `da75752`.

**VERIFIED USER-REPORTED EN DOCKER:**

1. En volúmenes previamente preparados, `prepare` repitió ocho resultados `READY`, `validate` reportó ocho `READY`, Web `/health/live` respondió HTTP 200.
2. Se reiniciaron ambos emuladores conservando volúmenes; ocho `READY` en `prepare` y `validate` y Web HTTP 200. Eso demuestra estructura persistida; **no** la conservación de documentos individuales.
3. En red y volúmenes nuevos aislados, la **nueva imagen** produjo ocho `CREATED` (Blob, base Cosmos y seis contenedores), salida 0, luego `validate` ocho `READY`.
4. Tras detener Cosmos, `validate` produjo Blob `READY`, base `FAILED` (`CosmosOperationError`), seis dependencias `BLOCKED`, resultado `PARTIAL`, salida 1 y evento de error estructurado.
5. Un nuevo job de preparación con Cosmos detenido emitió `ada.resource.preparation.unavailable`, salida 2. La Web previamente inicializada mantuvo `/health/live` en HTTP 200. Al restablecer Cosmos, `validate` volvió a ocho `READY` sin reiniciar Web.

El log de eventos por consola está probado; entrega efectiva a un sink Azure no está probada. El ensayo utiliza el artifact generado antes de que el usuario publicara el commit; la comparación estática indica que el commit incorpora los 12 archivos esperados, pero no equivale a repetir el ensayo con artifact reproducido desde HEAD.

## Fuera de alcance / OPEN

- `APPLICATION-RESOURCE-PLAN` completo y otros owners/resources de ADA o Atlanticus.
- Azure real: permisos, identidad, creación de contenedores, telemetría y observación del comportamiento productivo, **UNVERIFIED**.
- Persistencia de documentos de negocio y publicación/proyección end-to-end, **UNVERIFIED**.
- Contrato de disponibilidad funcional del Home: independiente del cierre de preparación; ver `04_WEB_READINESS_AND_DECOUPLING.md`.
- Desfase objetivo Python 3.14.7 frente a metadata/tooling 3.14.2: otro foco; no modificar aquí.

**Siguiente foco del proyecto:** Master Projection (`06_PRE_MANAGER_BOOTSTRAP_SURFACE.md`), no más cambios a Resource Preparation sin un finding nuevo.
