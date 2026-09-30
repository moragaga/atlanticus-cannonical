# ADA Command Center — Golden Path

Estado: **PARTIALLY IMPLEMENTED — C1/C2/C4, authoring UX-01/UX-02 y naming físico de Alarm Projection cerrados bajo sus gates. Recorrido completo con Resource Preparation/startup gate, Starter genérico, Runtime real y visualización operacional permanece NO ACREDITADO.** Corte: 2026-09-30.

## 1. Autoridad y límites del corte

```text
Implementation inspeccionada   moragaga/atlanticus:main@fe606cbefb932211b8329df9285004f4933df41d
Decisions inspeccionadas       moragaga/atlanticus-decisions:main@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical previo               moragaga/atlanticus-cannonical:main@c2f442b523fb4429f5a8a76c1e6919687016773f
```

La realidad implementada de este corte es `atlanticus:main`. Las pruebas mencionadas son evidencia local reportada por el usuario y no equivalen a CI, Docker ni Azure.

## 2. Recorrido actual, owners y gates

| Etapa | Owner actual | Estado demostrable |
|---|---|---|
| Tool Sources/Projections y conexiones nombradas | Tool/Web | CURRENT; qualification local controlada histórica, no Azure. |
| Confirmed Tool Catalog Cn | `web/tools/catalog` | CURRENT / C1 CLOSED en Blob. En host `local` todavía depende de Storage. |
| Discovery, inspect y confirmación humana | `web/tools/discovery-cosmos` | CURRENT / C1 CLOSED. |
| Tool Catalog UI reusable | `web/tools/catalog-manager` | CURRENT / C1 CLOSED. |
| Alarm editor Rn/Cn, Save/Validate/Publish | Alarm Web/Domain | Source v3 CURRENT; UX-01/UX-02 cerrados dentro de sus gates. |
| Source Key `alarm-configuration` única | Domain + Web/jobs | CURRENT / C2 CLOSED. |
| Alarm Projection physical name | Alarm Web/Projection | CURRENT / CLOSED: `alarm-configuration`; nombre largo anterior SUPERSEDED. |
| Alarm Projection local filesystem | Configuration Manager local | CURRENT: `conciencia_situacional/command-center/projections/alarm-configuration/...`. |
| Alarm Projection Cosmos | Web/Materialization | Contract CURRENT: container `alarm-configuration`, PK `/partition_key`; binding físico E2E UNVERIFIED. |
| `APPLICATION` común; leases separados y `VOLUMEN_PATH` manual | Procesos Alarm | CURRENT contractual; mismo volumen real multi-contenedor UNVERIFIED. |
| Qualification Rn/Cn | Materialization | JSON manual CURRENT; C3 productor automático no decidido. |
| B.2 READY/BLOCKED, pareja Runtime/Delivery exacta | Materialization | CURRENT. |
| WAL adopción → EFFECTIVE | Persistence/Runtime | CURRENT; Docker independiente UNVERIFIED. |
| Engine CURRENT v1 completo y FACTS v2 durables | Runtime | CURRENT. |
| Receptor Delivery último CURRENT con pin/READY/EFFECTIVE | `processes/alarms-delivery` | C4 CLOSED / CURRENT-only. |
| Resource Preparation + startup gate | Integración/Starter | **PLANNED / ÚNICO PRÓXIMO FOCO**. |
| Tool Catalog local filesystem | Web Tool Catalog | **NOT IMPLEMENTED**; bloqueo para considerar `local` completamente filesystem. |
| Starter genérico propio con runtime y Home mínima | Web | PLANNED después de cerrar la frontera de recursos/arranque. |
| Live materializer y `AlarmLiveProjection` | Backend Live | CONTRACT AGREED en Project, NOT IMPLEMENTED. |
| Management Capture/Projection y History/Analytics | Frentes independientes | PLANNED. |
| Docker/Azure real | Integración | UNVERIFIED. |

## 3. Contratos de cadena congelados

```text
Tool Sources/Projections
  -> confirmed Tool Catalog Cn
  -> Alarm Configuration Source Rn + ToolDependencyManifest Cn (v3)
  -> Alarm Projection
       local:   conciencia_situacional/command-center/projections/alarm-configuration/...
       durable: Cosmos container alarm-configuration, PK /partition_key
  -> Qualification vigente: intervención humana/JSON controlado
  -> Materialization: BLOCKED diagnóstico o READY pareja exacta
  -> Engine WAL adoption / EFFECTIVE (pin completo)
  -> Engine CURRENT v1 completo/reemplazable
  -> Engine FACTS v2 inmutables, canal separado
  -> Delivery input LAST CURRENT ONLY, pin/EFFECTIVE/READY exactos
  -> [NOT IMPLEMENTED] Live materializer + AlarmLiveProjection
  -> [PLANNED] Command Center Web operacional
```

Pin exacto: `source_key + result_id + manifest_sha256 + resolution_key`. Runtime/Delivery no sustituyen configuración por latest READY ni reinterpretan Rn/Cn desde latest Tool Catalog. READY no activa Engine. Web no procesa WAL ni resuelve priority/routing/cause por su cuenta.

La salida Runtime FACTS v2 permanece CURRENT aunque Delivery no la consuma como entrada.

## 4. Convención de identidad física cerrada en este hito

La aplicación constituye el boundary de su Cosmos. Por ello el resource physical name no repite aplicación/provider:

```text
CORRECTO
alarm-configuration

SUPERSEDED
ada-command-center-alarm-configuration-projection
```

El mismo physical name identifica el último nivel de la proyección local. No se mantiene alias/migración legacy.

La convención demostrada en este hito aplica a Alarm Configuration. No crear por inferencia containers o nombres para recursos todavía no contratados.

## 5. Evidencia local del hito

Commit integrado:

```text
fe606cbefb932211b8329df9285004f4933df41d
```

Pruebas aisladas:

```text
Alarm Configuration Web                         123 PASS
Alarm Projection Cosmos                           5 PASS
ADA Command Center Configuration Manager         28 PASS
TOTAL                                            156 PASS
git diff --check                                 PASS
```

La ejecución accidental de pytest global que recolectó cientos de paquetes fue inválida como gate del incremento y no se usa como evidencia.

## 6. Gate real aún no demostrado

No existe todavía un gate único que reúna:

```text
infra disponible
  -> resource preparation exitoso
  -> Web/procesos habilitados
  -> Tool real + catálogo confirmado
  -> Alarm Source/Projection durable
  -> qualification vigente
  -> READY
  -> EFFECTIVE
  -> Engine CURRENT
  -> Delivery CURRENT-only
  -> Live Projection
  -> lectura operacional por Home
```

El segmento `resource preparation -> startup gate` tampoco está implementado de forma genérica para Command Center. No inferirlo desde la existencia de stores ni desde la creación de directorios al escribir.

## 7. Siguiente frontera única

**PLANNED:** debate/diseño e implementación incremental de **Resource Preparation + startup gate**.

Objetivo del siguiente foco: auditar y reutilizar la infraestructura existente para resolver/asegurar recursos antes de iniciar consumidores, manteniendo la misma identidad lógica entre adapters locales y durables. Debe incluir el análisis del Tool Catalog local, porque hoy ese recurso sigue requiriendo Storage incluso en provider `local`.

No mezclar en ese incremento Starter Home, Live, Management, History/Analytics, nueva UX, C3 ni C5.
