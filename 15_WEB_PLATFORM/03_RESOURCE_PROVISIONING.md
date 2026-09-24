# Web Platform — Resource Provisioning

Estado: **CURRENT DIRECTION / MANAGER-SCOPED PLAN IMPLEMENTED / GLOBAL INVENTORY OPEN**

Checkpoint de implementación de este cierre: `moragaga/atlanticus@ce1213ec14cdee0be905c042c1cf513d71fb5b2d`.

## Principio congelado

Web orquesta la preparación de recursos de aplicación; Backend no debe provisionar ni verificar infraestructura en cada ejecución de job. El plan global `ApplicationResourcePlan` sigue **PLANNED / OPEN** hasta completar la traza real de componentes usados por cada Tool y aplicación:

```text
Tool/Application
→ Components
→ capabilities/runtimes
→ persistence/projections
→ resource requirements
→ container inventory
```

No extrapolar el plan parcial de ADA Generic Manager a Command Center, KPI delivery, alarmas, User Activity u otras aplicaciones. Congelar anticipadamente toda la inventario podría provocar contenedores innecesarios, particiones incorrectas, authorities duplicadas y acoplamiento prematuro.

## Capacidades reutilizadas — CURRENT

```text
WEB-STORAGE-TOPOLOGY             CLOSED / VERIFIED / CURRENT
USERS-STORAGE-TOPOLOGY           CLOSED / VERIFIED / CURRENT
STORAGE-PREFLIGHT-COSMOS-BRIDGE  CLOSED / VERIFIED / CURRENT
```

El bridge traduce `ResolvedStoragePlan` a `CosmosContainerSpec`, agrupa por `connection_ref` y ejecuta `ensure_containers` o `validate_containers` según la operación solicitada. No crea bases de datos, no recibe secretos y no posee readiness de aplicación. La creación local explícita de la base corresponde a `CosmosProvisioner`, invocado desde la composición ADA Generic.

## ADA Generic Manager — plan parcial implementado

Estado: **CURRENT / VERIFIED BY LOCAL AUTOMATED TESTS / REAL PROVIDERS UNVERIFIED**.

La aplicación utiliza en este frente una conexión y una base Cosmos. Los seis recursos lógicos/físicos del plan Manager son:

| Recurso lógico | Contenedor derivado del contrato | Partition key |
|---|---|---|
| `navigation.projection` | `navigation-projection` | `/partition_key` |
| `users.support` | `users-support` | `/partition_key` |
| `users.runtime` | `users-runtime` | `/id` |
| `ada.tools.projection` | `ada-tool-projection` | `/partition_key` |
| `ada.kpis.registry.projection` | `ada-kpi-registry-projection` | `/partition_key` |
| `ada.kpis.definition.projection` | `ada-kpi-definition-projection` | `/partition_key` |

Los contratos vigentes declaran `default_ttl_seconds=None` para esos seis recursos. Navigation conserva su contenedor propio. Profiles y ADA Access son consumidores compatibles de `users-support`; no constituyen un séptimo u octavo contenedor. `users-runtime` conserva `/id` y no se fusiona con `users-support`.

La composición se encuentra en:

```text
scopes/ada/web/application/ada-generic-application/
  src/ada/web/application/generic/manager_persistence.py
  src/ada/web/application/generic/manager_deployment.py
```

La validación rechaza conexiones Cosmos divergentes dentro del plan y cambios físicos incompatibles. Los nombres de contenedores Cosmos no se solicitan en `.env`: cada capability los declara en su contrato. Esta restricción de conexión única corresponde **a ADA en esta etapa**, no al núcleo reutilizable de Atlanticus, que admite conexiones múltiples nombradas.

## Storage Blob — CURRENT

El único nombre físico de contenedor configurable en este arranque es el Blob compartido, mediante `ADA_TOOL_SOURCE_BLOB_CONTAINER_NAME`. La connection string identifica la cuenta, **no el contenedor**; alternativamente se admite cuenta/SAS conforme al contrato de Storage. El contenedor Blob debe existir antes del comando de preparación actual; el conector usado por este flujo valida su presencia, pero **no lo crea**.

Dentro del contenedor físico compartido:

```text
<application_namespace>/sources/...               # Navigation, Profiles y ADA Access
<application_namespace>/users/users.json.gz         # Users Registry global
<application_namespace>/<tool_namespace>/sources/... # Tools, KPI Registry/Definition
```

`BlobSourceStore` agrega internamente `sources/<SourceKey codificado>/...`; la composición sólo le entrega el prefijo lógico global o de herramienta. Los SourceKey se codifican mediante Base64 URL-safe. No duplicar `sources` en el prefijo.

## Preparación explícita — CURRENT

```text
uv run ada-generic-manager-resources ensure-local
```

En `local`, el CLI verifica primero el Blob preexistente, asegura la base Cosmos y asegura los seis contenedores del plan Manager. `ensure-local` está prohibido en `production`.

```text
uv run ada-generic-manager-resources validate
```

Verifica el Blob y la existencia/topología de los seis contenedores Cosmos sin mutarlos. Si partition key o TTL difieren de lo declarado, la operación falla; no modifica automáticamente recursos incompatibles. Construir clientes/stores no ejecuta preflight de red de manera implícita.

**Límite:** estos comandos y el plan fueron verificados con pruebas automatizadas locales/simuladas, no con el conjunto real de emuladores ni con Azure. No equivalen al aprovisionamiento general de la aplicación ni incluyen, por inferencia, los contenedores de KPI Delivery o alarmas.

## Cloud — dirección vigente, no implementación cerrada

```text
infraestructura base / IaC
→ Web
→ validate / ensure de recursos permitidos
→ projection bootstrap
→ Backend
```

En Cloud, la cuenta y la base son preexistentes. Los permisos para crear recursos, el inventario global y la superficie de readiness permanecen pendientes. La ejecución automática de `ensure-local` no se habilita en Cloud.

## OPEN

- `APPLICATION-RESOURCE-PLAN`: inventario por Tool/Application, incluyendo Command Center, KPI pipeline, alarmas, User Activity y Source/Blob.
- Paridad de creación de contenedores Blob para bootstrap local limpio.
- Ownership, criticidad y conexión de recursos fuera del plan parcial Manager.
- Permisos Cloud de preparación; readiness `READY/DEGRADED/ERROR` y punto integral de orquestación Web.
- Qualification con proveedores reales, reinicio, publicación/proyección y recuperación.
