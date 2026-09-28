# Web Platform — Projection Orchestration

Estado: **EXACT TARGET CONTRACT CURRENT / USERS SPECIAL CURRENT EN SU ALCANCE / MASTER 001B PLAN READ-ONLY CURRENT / MASTER 001C HTTP CURRENT / 001D APPLY PLANNED**  
Corte estático `atlanticus@94f26213ca28b550baf53d8ee34e34da7538ad17`; canonical anterior `ed1edd7a04533cf32a2b32844cb79b9edbe7bcc8`. No trasladar qualification histórica de otros SHAs al actual.

## Contrato ordinario CURRENT

No introducir un orden global artificial. Las dependencias semánticas de la identidad exacta son `ProjectionTarget.dependencies`: conservar objetos `ProjectionTarget` completos y normalizados. Dependencias de lectura se resuelven sin crear ciclos ficticios entre Sources. Reintentar un mismo target exacto no debe crear otra identidad funcional ni reproyectar por comodidad cuando el contrato permite reconocer alineación.

Navigation y Tools pueden tener Sources independientes. ADA Access depende del Profiles ProjectionTarget; KPI Registry depende de Tool; KPI Definitions depende de KPI Registry. Un string privado de revision no sustituye targets dependientes completos.

## Planner Master CURRENT — inspección solamente

Código implementado:

```text
scopes/ada/web/application/ada-generic-application/src/ada/web/application/generic/master_projection/plan.py
scopes/ada/web/application/ada-generic-application/src/ada/web/application/generic/master_projection/composition.py
```

La composición utiliza exactamente los seis pares disponibles de `ConfigurationManagerStores`:

| Par | Prerrequisito registrado en esta composición |
|---|---|
| Navigation Source → Projection | Ninguno del resto de estos seis |
| Profiles Source → Projection | Ninguno del resto de estos seis |
| Tools Source → Projection | Ninguno del resto de estos seis |
| ADA Access Source → Projection | Profiles |
| KPI Registry Source → Projection | Tools |
| KPI Definitions Source → Projection | KPI Registry |

El planner reporta `SOURCE_MISSING`, `CURRENT`, `NEVER_PROJECTED`, `OUTDATED`, `BLOCKED` o `UNAVAILABLE`. Los campos incluyen Source release, target actual/ya proyectado, prerequisitos, bloqueos y tipo de error. `MasterProjectionPlan.to_dict()` expone `mode: READ_ONLY`, `entries` y `ready_source_keys`. `ready` únicamente señala entradas que el planner observa como no proyectadas/desactualizadas. **No** constituye un permiso ni un plan de ejecución durable garantizado frente a carreras o cambios posteriores de Source.

La UI `/master-projection` requiere material/credenciales de servicio válidas y **solo muestra** el plan; no tiene endpoint/botón `projection.apply` ni acciones de Users REPLACE implementadas.

## Users — excepción administrativa CURRENT separada

Servicios existentes de Users:

```text
UsersAdministrationService: discover / promote / update
UsersApprovedRecoveryService: preview_capture / capture / validate / restore
                              validate_replace / replace_approved
```

Solo snapshots **expresamente aprobados** pueden usarse como base de restauración/sustitución; un Registry Blob también contiene candidatos. `validate_replace` prepara la comparación; `replace_approved` reconcilia Blob/Cosmos con before-image y auditoría conforme a su contrato existente. No suponer atomicidad distribuida. Esta operación no es el séptimo par Source/Projection ordinario ni un módulo sintético del Coordinator.

La página **Proyección de usuarios** del Manager requiere `users.manage` y tiene las pestañas Crear respaldo / Proyectar usuarios; no sirve como autorización Master pre-Manager. El planner externo Master presenta por separado los estados `CATALOG_UNAVAILABLE`, `SNAPSHOT_MISSING`, `PROFILES_PENDING`, `SNAPSHOT_SELECTION_REQUIRED`, y marca `executable: false`. El provider durable del Manager deriva `identity_realm` del único issuer entre promovidos; destino vacío y transporte interambientes permanecen como gates explícitos.

## Master 001D — frontera PLANNED, sin contrato de escritura congelado

`projection.apply` consta como **acción declarada** en el formato de material 001A, pero no tiene controlador implementado en 001C. En el siguiente debate se debe inspeccionar, no inventar:

1. Puertos/servicios de cada par, modo de selección y evaluación de `ProjectionTarget`, Stores existentes y precondiciones de concurrencia.
2. Autorización por operación en servidor: identidad del material, namespace, environment, fingerprint, sesión vigente y permiso declarado; la UI no es autoridad.
3. Validación nuevamente actualizada justo antes de cualquier escritura y confirmación explícita de un operador; no reutilizar sin más un preview antiguo.
4. Semántica idempotente compatible con targets exactos, resultados por componente, errores parciales, reintentos y auditoría usando capacidades realmente presentes.
5. Límites de escritura respecto del Manager y la separación de backend/frontend.

La lista anterior es **agenda de diseño, no endpoints, DTOs, mecanismos de auditoría ni algoritmo aprobados**. No escribir código nuevo ni crear procesos o compatibilidad legacy hasta alcanzar consenso. `users.replace` se mantiene fuera del primer alcance 001D mientras su destino vacío, seguridad y recuperación real sean OPEN.

## Frentes independientes

La identificación organizacional ADA (cargo, área operativa, grupo) no se incorpora a Atlanticus Users ni a este planner; los problemas de Home, KPI collector, Command Center y alarmas se documentan en sus propias secciones. La excepción Master de credencial de servicio frente a la baseline histórica Entra pre-Manager requiere resolución contractual expresa antes de producción.
