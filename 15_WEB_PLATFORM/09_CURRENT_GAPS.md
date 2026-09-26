# Web Platform — Current Gaps

Estado: **CURRENT / ADA GENERIC + NAVIGATION LOCAL CLOSED / REAL DISTRIBUTION OPEN**

Implementación para este delta: `moragaga/atlanticus@a6061ffed59c8b04e64b0a7fdc17050ef463c850`.
Canonical previo inspeccionado: `55c531b192fedcc6343b3c9e2ee1f9ec4ffa8fab`.

## CLOSED en el alcance implementado/local

```text
ADA-STORAGE-NAMESPACE
TOOL-PROJECTION-PERSISTENCE
TOOL-PERSISTENCE-RESILIENT-COMPOSITION
ADA-GENERIC-OPERATIONAL-BOOTSTRAP
ADA-WEB-KPI-COLLECTOR-OPERATIONAL-ATTACHMENT WHEN TOOL READY
ADA-GENERIC-INTEGRATED-MANAGER-LOCAL-BOOTSTRAP
ADA-GENERIC-MANAGER-DURABLE-ADAPTER-COMPOSITION (STATIC/TESTED PREVIOUSLY)
ADA-GENERIC-MANAGER-RESOURCE-CLI CONTRACT
ADA-GENERIC-COSMOS-CONTAINER-ENV-CUTOVER
ADA-GENERIC-NAVIGATION-INTEGRATION
ADA-GENERIC-NAVIGATION-LOCAL-PUBLISH-PROJECT-CONSUME
ADA-GENERIC-NAVIGATION-CLIENT-CORRECTION (MANUAL)
```

La qualification local de Navigation no cambia el estado de los recursos productivos.
Evidencia previamente reportada: integración ADA Generic `169 passed` y Ruff verde;
posterior correctivo `172 passed` y Ruff verde; shell `8 passed`, `1 skipped` y fallo
Ruff en el test nuevo previo a `a6061ffe`. No extrapolar esos valores al commit final.

## Implementación que no debe reconstruirse

- `AdaGenericSettings`: Source `local|blob` y Projection `local|cosmos` independientes.
- Cosmos container names se derivan de contratos; no reintroducir container names en `.env`.
- Blob usa `ADA_TOOL_SOURCE_BLOB_CONTAINER_NAME` y credenciales separadas por contrato.
- Manager selector `auto|local|durable|disabled`.
- Durable Manager reutiliza Tool Blob/Tool Cosmos bajo la composición actual; `__main__.py`
  limita durable local a `LocalIdentityProvider` y requiere host externo para producción.
- `ada-generic-manager-resources ensure-local|validate` existe; `ensure-local` exige que
  Blob ya exista y prepara/verifica sólo los recursos permitidos por el contrato.
- Navigation consume su proyección compartida con Manager y tiene excepción administrativa
  explícita según `administrative_override`; no es un acceso general a Manager.

## OPEN

| Elemento | Estado | Razón |
|---|---|---|
| ADA-GENERIC-DOCKER-REAL-PERSISTENCE-QUALIFICATION | PLANNED / UNVERIFIED | No hay evidencia aquí de workflow completo sobre Blob/Cosmos emulados/reales y reinicio. |
| ADA-GENERIC-WEB-DISTRIBUTABLE-ARTIFACT | PLANNED / UNVERIFIED | Build/deploy portable fuera del checkout no cualificado. |
| Blob local preprovisioning | OPEN | CLI Manager comprueba Blob; no lo crea. |
| AccessRuntime Identity/Manager | UNVERIFIED | La observación estática histórica de instancias separadas requiere revalidación específica tras integración. No atribuir fallo no observado. |
| ApplicationResourcePlan global/readiness | PLANNED / OPEN | No confundir plan parcial Manager con inventario global. |
| Entra/Graph host productivo | PLANNED / UNVERIFIED | No se probó ni se asumió un proveedor real. |
| Python metadata 3.14.7 | PLANNED / CONFLICT | La definición distribuible inspeccionada usa `3.14.2`. |
| CI remoto, workspace Ruff y monorepo tests | UNVERIFIED | No hay salida del HEAD final para esas gates. |
| Navigation authorization consumer | CLOSED en el alcance actual | Código y tests de integración presentes; no conservar el viejo BLOCKED de canonical sin reevaluación. |

## Regla

Calificar cada frontera por su evidencia. No sustituir los stores durable o los credenciales
reales por los stores in-memory de la prueba local, ni introducir adaptadores temporales.
