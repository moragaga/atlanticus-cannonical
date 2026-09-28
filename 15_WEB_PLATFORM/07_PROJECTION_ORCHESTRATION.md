# Web Platform — Projection Orchestration

Estado: **CURRENT CONTRACT / ISOLATED BOOTSTRAP EXTENSION PLANNED**

## Regla principal CURRENT

No existe un orden global artificial entre todas las proyecciones. Las dependencias semánticas reales que forman parte de la identidad exacta de una Projection se declaran mediante `ProjectionTarget.dependencies`. Las dependencias que son sólo composición de lectura permanecen en resoluciones derivadas. No generar ciclos `Projection A -> modifica Source B -> Projection B -> modifica Source A`.

Ejemplos conceptuales de base independientes, cuando su contrato lo permita:

```text
Navigation Source -> Navigation Projection
Tool Source       -> Tool Projection
```

Ejemplo de dependencia real implementada: el target de KPI Configuration conserva el `Tool ProjectionTarget` exacto. No releer un Tool CURRENT cambiante durante la proyección. KPI Definition y otras relaciones deben respetar sus contratos implementados al momento de ejecutar; no sustituir targets exactos por revision strings privadas.

`ProjectionTarget.dependencies` mantiene `ProjectionTarget` completos, prohíbe referencia al mismo `source_key` y duplicaciones, y normaliza determinísticamente las dependencias. La orquestación decide cuándo lanzar una proyección, **no inventa** su identidad contractual.

Proyectar nuevamente el mismo target exacto debe conservar identidad efectiva o resultar en un no-op equivalente conforme al contrato del dominio; no fabricar revisiones funcionales nuevas sólo por reintentar.

## Nuevas fronteras acordadas (PLANNED, no implementadas)

### Users es excepción administrativa explícita

Users CURRENT es `ManagerEntry` con operaciones `discover/promote/update`, Blob `UsersRegistryStore` y Cosmos `UsersAdministrationStore/UsersRuntimeStore`. **No** tiene `ManagerModule` Source/Projection ni una operación integral ya implementada para reconciliar usuarios. No fingir que el ejemplo histórico `Users Source -> Users Projection` es un workflow del Coordinator hoy disponible.

El nuevo proceso especial deberá identificar el conjunto **efectivamente aprobado** de usuarios desde Storage; validar diferencias contra Cosmos y reconstruir la proyección con confirmación explícita. No promover candidatos por la sola presencia en el registro. No usar cambios directos en Cosmos como autoridad del Source. Diseño próximo: `13_USERS_PROJECTION_RECOVERY.md`.

### Página de proyección independiente

La página aislada utilizará los proyectores existentes y el nuevo proceso especial de Users. No será Manager, no dará sus permisos y no volverá a implementar reglas de dominio. Se proyectarán los Sources disponibles en Storage para el ambiente, verificando dependencias reales. Registro de resultados parciales y reintentos: contrato pendiente de revisión sobre APIs existentes, no promesa de transacción entre Blob/Cosmos.

### Identificación operacional ADA

El módulo nuevo de ADA tendrá Source/Projection propios por definir y consumirá identidades existentes ya promovidas. Sus proyecciones se incorporarán posteriormente a la página aislada cuando su contrato esté implementado. `area` (Mina/Planta/null), cargo de catálogo manual y grupo (1-4/null) no intervienen en autorización y no se agregan a Atlanticus Users.

## Refinamiento respecto de Baseline 1.0

La dirección general `no artificial bootstrap dependencies + exact dependencies when real + derived resolutions only when genuinely derived` se conserva. Queda refinado el ejemplo de Users: **el workflow especial no está implementado**, y debe diseñarse a partir del registro y stores existentes, no mediante adaptación legacy para aparentar simetría con otros Managers.

No implementar ninguna de estas extensiones durante el cierre documental.
