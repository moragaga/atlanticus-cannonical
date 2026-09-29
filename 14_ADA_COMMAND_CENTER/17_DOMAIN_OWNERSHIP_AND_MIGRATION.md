# ADA Command Center — Domain Ownership and Migration

Estado: **CURRENT / C1 WEB TOOLS CLOSED estructuralmente; domain/alarms y domain/tools CURRENT; normalización transversal diferida**. Checkpoint: `atlanticus:main@3961385aecd0eb7e373018fc25e509a71dccc409`, 2026-09-29.

## Alarm authored domain — CURRENT

```text
scopes/ada-command-center/domain/alarms
ada-command-center-alarms-domain==1.0.0
```

Owns Alarm authoring contracts and `AlarmConfigurationSnapshot` v3; no posee UI Tool, Blob persistence ni jobs.

## Command Center Tools shared domain — CURRENT

```text
scopes/ada-command-center/domain/tools
ada-command-center-tools-domain==1.0.0

ToolDependencyEntry
ToolDependencyManifest
```

Contrato transversal compartido por publicación/historia Alarm y Materialization. No trasladar consolidación ni adapters físicos al dominio por la sola razón de estar implementados en Python.

## Dependency direction CURRENT / límite no resuelto

```text
ada-web-tools structural contracts
        ↓
domain/tools
        ↓
domain/alarms snapshot wrapper
```

`domain/alarms` no tiene `dependencies=[]`. La normalización transversal de tipos estructurales que hoy viven en `ada-web-tools` continúa `PLANNED / DEFERRED`. C1 no la ejecutó y no debe introducir una refactorización incidental.

## Tool Catalog / Discovery / UI — CURRENT tras C1

```text
scopes/ada-command-center/web/tools/catalog
    ada-command-center-web-tool-catalog==0.1.0
    snapshots, codecs, consolidación y persistencia Blob CURRENT

scopes/ada-command-center/web/tools/discovery-cosmos
    ada-command-center-web-tool-discovery-cosmos==0.1.0
    discovery, conexiones nombradas, inspect/confirm/adopted

scopes/ada-command-center/web/tools/catalog-manager
    ada-command-center-web-tool-catalog-manager==0.1.0
    UI y callbacks Manager capability-local
```

`backend/tools/catalog`, `backend/tools/discovery-cosmos` y los antiguos namespaces/distribuciones son **SUPERSEDED / ELIMINADOS de Git en C1**; no reinstalar aliases, shims ni implementaciones duplicadas. El host temporal `web/application/ada-command-center-configuration-manager` compone dependencias, principal, providers y conexión física; la UI de Tool Catalog ya no reside en él.

**Regla de ownership acordada:** en ADA Command Center, `backend` organiza trabajos ejecutables y lógica específica de sus procesos. La lógica Python server-side usada exclusivamente por Web pertenece a Web, independientemente de que use Cosmos/Blob. Cuando un contrato es genuinamente compartido y se demuestra la frontera transversal, definir su owner neutral/Domain mínimo; Web no se reduce a HTML, CSS, JavaScript y Dash.

## Backend Alarm — CURRENT

```text
scopes/ada-command-center/backend/alarms/materialization
```

Owner de resolver B.2 puro, contratos y lector exacto; no adquiere automáticamente responsabilidad de Source/Projection Web ni de UI.

```text
scopes/ada-command-center/backend/processes/alarms-materialization
scopes/ada-command-center/backend/processes/alarms-runtime
scopes/ada-command-center/backend/processes/alarms-delivery
```

Procesos operacionales existentes. El lock redundante individual de `processes/alarms-materialization/uv.lock` se retiró en C1; workspace `backend/uv.lock` permanece. **OPEN separadamente:** el proceso Materialization importa hoy el adaptador de infraestructura `web/alarms/projection-cosmos`. No convertir esa dependencia actual en mandato de mover todo a Domain ni resolverla silenciosamente durante C2.

## Alarm Source/Projection y composición Web

`web/alarms/configuration` posee Source codec/workflow, correlación workspace y builder base. `web/alarms/persistence`, `web/alarms/projection-local` y `web/alarms/projection-cosmos` exponen adapters ya existentes. El Manager temporal compone Source/Projection y los Tool services. Estas piezas no constituyen el Starter distribuible de Command Center.

## Próximas fronteras acordadas sin implementación

- `C2 PLANNED`: tres jobs con APPLICATION/rutas y Source Key comunes; contenedor Cosmos físico derivado de un resource contract, nombre de contenedor Blob sí configurable. Debatir owner exacto y compatibilidad real antes de tocar consumidores.
- `C3 PLANNED`: qualification real programática; productor y verificadores GREEN siguen OPEN.
- `C4 PLANNED`: Delivery consume sólo último CURRENT, no historial FACTS backlog; Runtime sigue produciendo FACTS durables.
- `C5 PLANNED`: evidencia técnica no parametrizada artificialmente y `.env.detail` consistente tras verificar contratos.
- `Starter PLANNED`: `ada-command-center-generic` propio, no heredar automáticamente ADA Generic ni acoplar Atlanticus a ADA.

## No legacy

No aliases/shims para Source schema v2, snapshot antiguo, modelos authored duplicados ni namespaces de Tool anteriores. Respetar frozen Manager global: navegación, Home, workflow y permisos siguen bajo la capacidad genérica Manager; los módulos poseen su formulario y lógica específica.

## Evidencia / límites

C1 fue verificado en Git remoto y gates locales del usuario (tests, Ruff, wheels e importaciones de host). `UNVERIFIED`: instalación wheel aislada, Web/browser final, Azure/CI completa y Starter. No transformar esos pendientes en trabajos iniciados por inferencia.
