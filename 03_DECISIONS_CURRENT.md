# Atlanticus — Current Decisions

Estado: **CURRENT**

| Decisión | Estado |
|---|---|
| Python 3.14.7 | DECIDED / NOT YET IMPLEMENTED |
| `python:3.14.7-slim-trixie` | DECIDED / NOT YET IMPLEMENTED |
| `uv`, no pip normal | CURRENT |
| `backend/` representa backend jobs; no todo Python server-side | CURRENT |
| Server-side Python con responsabilidad Web pertenece a `web/` | CURRENT |
| Connectivity es dual-use y no adquiere ownership funcional | CURRENT |
| Manager posee shell/header administrativo propio | CURRENT |
| ADA Generic usa shell/header operacional ADA | CURRENT |
| Manager ≠ ADA operational shell | CURRENT |
| Source genérico pertenece a `web/capabilities/source` | CURRENT |
| Blob será Source durable productivo en dominios migrados | DECIDED / NEXT |
| Local y Blob deben compartir semántica Source | FROZEN |
| Releases Source son publicaciones completas e inmutables | FROZEN |
| `release_id` identifica publicación y no equivale a `content_hash` | FROZEN |
| Dos releases pueden compartir `content_hash` | FROZEN |
| `manifest.json` es el único commit point de publicación | FROZEN |
| Draft no crea Source release | CURRENT |
| Restore histórico crea release nuevo | FROZEN |
| Restore nunca repunta current directamente a un release histórico | FROZEN |
| `previous_published_release` y `basis_release` son conceptos distintos | FROZEN |
| `ConcurrencyToken` es opaco para consumidores | FROZEN |
| El provider Source aplica la precondición autoritativa de concurrencia | FROZEN |
| No existe bypass `force=True` de concurrencia | FROZEN |
| History funcional sigue sólo la cadena de publicaciones alcanzable desde current | FROZEN |
| Un candidato que pierde CAS es orphan y no History | FROZEN |
| SourceStore publica cuando se le ordena; el no-op por mismo hash pertenece al consumidor | FROZEN |
| Projection identifica `source_release_id` concreto | DECIDED / NOT YET IMPLEMENTED |
| Projection failure no revierte Source | FROZEN |
| Cosmos nunca determina Source current | FROZEN |
| Tool Configuration determina existencia estructural | FROZEN |
| Data determina estado | FROZEN |
| Component = Store + Collector contract + KPI destination | FROZEN |
| Subcomponent no crea Store/Collector propio | FROZEN |
| Data granularity != Alarm visual granularity | FROZEN |
| ADA Generic integra configuración/capacidades y habilita Tool específica | CURRENT DIRECTION |
| No construir capability `collectors` sin mapear producers/sources actuales | CURRENT |
| Tests sin validación CSS visual | CURRENT |
| No tests de existencia/no existencia de funciones internas | CURRENT |
| Procesos manuales/semi-automáticos son válidos | CURRENT |
| Alarm Definition B.1 | DESIGN FROZEN |
| Alarm B.2 Live != Management | DECISION RECORDED |
| Alarm B.2 I1 publication/runtime/delivery | DECISION RECORDED |
| Alarm B.2 I2 `LATEST SAVED = LATEST VALID` | DECISION RECORDED |
| R3.5 Alarm final qualification | CLOSED PASS/GREEN |

## Source checkpoint

```text
SOURCE-1A.1
Core + Local
CLOSED / VERIFIED
```

Implementación actual:

```text
web/capabilities/source/core
web/capabilities/source/local
```

Blob es el siguiente provider.

No modificar `connectivity/storage` salvo que el provider Blob demuestre una carencia técnica real del contrato de conectividad.

## SharePoint

SharePoint aparece en contratos históricos de Manager/Alarm y en adapters Source legacy.

No retirar todavía esos adapters.

Clasificar:
- semántica reusable → conservar;
- storage binding SharePoint → candidato a `SUPERSEDED` tras Blob parity;
- Power Automate Source pipeline → objetivo de retiro posterior a validación.

Cuando una migración autorice retiro, el incremento debe enumerar rutas exactas a borrar y validar gates después de la eliminación.

## Entrega

La prioridad es integración vertical hacia un entregable usable.

No optimizar roadmap por orden histórico de incrementos si existe un camino más corto y defendible al primer producto integrado.

## ADA Command Center

- Command Center Web propia: `DECIDED / NOT YET IMPLEMENTED`.
- Alarm Engine pertenece funcionalmente a Command Center: `CURRENT`.
- Command Center no authoring de Tools: `CURRENT DIRECTION`.
- Tool topology se consume confirmada/read-only: `FROZEN SEMANTICS`.
- Configuration incluye Alarm Rules + Message Catalog: `CURRENT DIRECTION`.
- Entra ID + Navigation + Profiles: `CURRENT DIRECTION`.
- Generic Actions, User Activity y app/session auto-refresh: `OUT OF SCOPE INITIAL`.
- Live Projection != Management Projection != History/Analytics purpose.
- Dashboard debe producir conclusiones trazables, no métricas decorativas.

## Web Platform / Deployment

- Users/Profile, Navigation y User Activity deben poder instalarse independientemente: `CURRENT DIRECTION`.
- Cross-capability binding pertenece a composition/adapters: `CURRENT DIRECTION`.
- ADA Manager Users→Navigation direct coupling debe retirarse: `IDENTIFIED GAP`.
- User Activity debe conservar historia ordenada por página/visita: `CURRENT DIRECTION`.
- User Activity TTL = 24 h / 86400 s: `CURRENT`.
- No guardar cada heartbeat como historia: `CURRENT DIRECTION`.
- Dashboard agrega datos sin convertir producers en dependencias mutuas: `CURRENT DIRECTION`.
- Web es startup/resource/projection orchestrator: `CURRENT DIRECTION`.
- Web no es runtime coordinator de Backend jobs: `CURRENT`.
- Web debe existir sin datos/backend y con integrations opcionales ausentes: `CURRENT DIRECTION`.
- Cosmos database puede crearse localmente por bootstrap Web: `CURRENT DIRECTION`.
- Cosmos database productiva debe preexistir; Web no la crea: `CURRENT DIRECTION`.
- Web prepara/valida containers productivos dentro de permisos: `CURRENT DIRECTION`.
- Partition key/TTL mismatch nunca se corrige silenciosamente: `CURRENT`.
- Backend no repite provisioning validation en cada job execution: `CURRENT DIRECTION`.
- Orden de despliegue de aplicación: Web → preparación/proyección → Backend: `CURRENT DIRECTION`.
- Retirar `is_local → full Manager access`: `CURRENT DIRECTION`.
- Crear superficie pre-Manager independiente de Users/Profile projection: `CURRENT DIRECTION`.
- Superficie pre-Manager no implica mutación anónima en producción: `CURRENT`.
- Projection bootstrap debe ordenar por dependencias declaradas, no lista global rígida: `CONTRACT DESIGN`.

## KPI repair / reprocessing

- El pipeline KPI necesita reproceso del estado current sin esperar data nueva: `CURRENT DIRECTION`.
- No usar una flag genérica `DEBUG_MODE`: `CURRENT DIRECTION`.
- Flags de reproceso son por proceso y default false: `PROPOSED`.
- KPI Runtime puede reevaluar el mismo source/committed watermark: `PROPOSED`.
- Same watermark no autoriza overwrite durable conflict: `CURRENT`.
- Latest Delivery puede republicar current ignorando checkpoint current: `PROPOSED`.
- Historian repair reconstruye desde durable evaluations hasta committed: `PROPOSED`.
- Timeseries Delivery puede republicar current desde Historian authority: `PROPOSED`.
- Reprocess jamás permite watermark regression: `CURRENT`.
- Lease/fencing/cancellation permanecen activos: `CURRENT`.

## Bootstrap closure

- Canonical design bootstrap closes at Baseline 1.0.
- Open items remain explicit and do not justify more generic architecture before product execution.
- First Tool: `Operaciones Integradas`.
- Second Tool: `Mina`.
- Command Center analytics is deferred until data sufficiency tests.
- Base projections are independent; only derived resolutions express dependencies.
- Containers/resource inventory remains pending until Tool/component trace is complete.
- Pre-Manager surface is a real authenticated Login/Bootstrap Console.
- Backend and frontend generators target distribution-ready artifacts; Atlanticus does not own corporate DevOps pipeline implementation.
- Component validation scripts continue; a master integrity gate aggregates them.
- Cosmos and Storage are primary supporting services and should be declarable in distribution contracts.
- env.detail becomes explanatory documentation, not just example values.
- Stable components require real READMEs.
- Loaders are product requirements.
- ADA Manager Component External Links use stable Tool/Component identity, JS-controlled popover and warmup/cache.
- University use cases are pedagogical artifacts, not tests.
