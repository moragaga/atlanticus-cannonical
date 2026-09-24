# Web Platform — Current Gaps

Estado: **CURRENT CHECKPOINT / ADA GENERIC LOCAL INCREMENT CLOSED / REAL PROVIDER QUALIFICATION OPEN**

Autoridad de implementación inspeccionada: `moragaga/atlanticus@ce1213ec14cdee0be905c042c1cf513d71fb5b2d`.

Referencia documental previa inspeccionada: `moragaga/atlanticus-cannonical@6bd7f1f2616f954b422f3ddc1549a53a9b479682`.

## CLOSED / VERIFIED / CURRENT en el alcance local

```text
ADA-STORAGE-NAMESPACE
TOOL-PROJECTION-PERSISTENCE
TOOL-PERSISTENCE-RESILIENT-COMPOSITION
ADA-GENERIC-OPERATIONAL-BOOTSTRAP (COMPOSITION)
ADA-WEB-KPI-COLLECTOR-OPERATIONAL-ATTACHMENT (WHEN TOOL READY)
ADA-GENERIC-INTEGRATED-MANAGER-LOCAL-BOOTSTRAP
ADA-GENERIC-MANAGER-DURABLE-ADAPTER-COMPOSITION
ADA-GENERIC-MANAGER-RESOURCE-CLI CONTRACT
ADA-GENERIC-COSMOS-CONTAINER-ENV-CUTOVER
```

Evidencia aportada por el usuario tras el correctivo final: `uv lock --check`, Ruff check, Ruff format check, **157 tests aprobados**, mirrors validados y wheel `ada_generic_application-0.2.17-py3-none-any.whl` construido. Son pruebas **locales de ADA Generic**, no integración real Azure/Docker ni qualification transversal.

## Implementación CURRENT: lo que NO debe rehacerse

- `AdaGenericSettings` resuelve `ToolSourceProvider` (`local|blob`) y `ToolProjectionProvider` (`local|cosmos`) por separado.
- Los nombres de contenedores **Cosmos** derivan de los contratos internos. Variables Cosmos de contenedor fueron eliminadas del `.env` de ADA Generic, incluidas Latest/Timeseries en el collector reader.
- Blob mantiene `ADA_TOOL_SOURCE_BLOB_CONTAINER_NAME`, separado de sus credenciales (connection string o SAS).
- Manager usa selector `auto|local|durable|disabled`; en este hito `durable` exige Tool Blob + Cosmos y reutiliza esas conexiones, con una base Cosmos ADA.
- `ManagerPersistenceResources` agrupa un contenedor Blob físico para Sources y Users Registry, con prefijos globales/por Tool; seis contenedores Cosmos para Manager, incluyendo `users-support` compartido por Profiles y Access y Navigation separado.
- Existe CLI explícito `ada-generic-manager-resources ensure-local|validate`. `ensure-local` requiere Blob preexistente y sólo puede crear/verificar Cosmos en `local`.
- El ejecutable productivo `ada-generic-application` **no** activa Manager durable sin proveedor de identidad productivo externo; su CLI local utiliza Jane/John.

## OPEN / por qué

| Elemento | Estado | Motivo |
|---|---|---|
| ADA-GENERIC-DOCKER-REAL-PERSISTENCE-QUALIFICATION | PLANNED / NEXT / UNVERIFIED | No se ejecutó arranque y workflow completo con Cosmos/Blob reales/emulados, publicación, proyección y recovery. |
| Blob local bootstrap sin precreación manual | OPEN | El CLI actual sólo verifica contenedor Blob. |
| Cohesión `AccessRuntime` Manager/Identity | OPEN / VERIFIED STATIC / RUNTIME UNVERIFIED | Código construye dos instancias independientes; falta prueba de coherencia efectiva. |
| `ApplicationResourcePlan` global y readiness integral | PLANNED | Existe sólo plan parcial Manager; no incluye todos los consumidores. |
| Entra/Graph concreto productivo | PLANNED / UNVERIFIED | El host debe inyectar proveedor real; no implementado/cualificado en este corte. |
| Alineamiento metadata Python 3.14.7 | PLANNED / SEPARATE | `requires-python ==3.14.2` todavía figura en paquetes del alcance. |
| CI remoto / Ruff workspace / pytest monorepo | UNVERIFIED | No hay salida de ejecución de estas pruebas. |
| Navigation Manager authorization consumer alignment | BLOCKED / SEPARATE | Conflicto histórico `can_view` vs `can_access` registrado; no corregido en este hito. |

## Único siguiente foco

**ADA-GENERIC-DOCKER-REAL-PERSISTENCE-QUALIFICATION**: calificar el flujo existente con recursos locales reales/emulados. Comenzar por verificar comandos y recursos implementados; probar estado sin datos, publicación/proyección, reinicio y autorización. No rediseñar capabilities, no mezclar alarmas/Command Center, ni declarar producción lista a partir de tests simulados.
