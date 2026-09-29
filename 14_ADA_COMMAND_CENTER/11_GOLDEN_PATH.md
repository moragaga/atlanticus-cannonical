# ADA Command Center — Golden Path

Estado: **PARTIALLY IMPLEMENTED — C1/C2/C4 y authoring UX-01/UX-02 cerrados bajo sus propios gates; recorrido completo con Starter genérico + Runtime real + visualización operacional NO ACREDITADO.** Corte UX: 2026-09-29.

## 1. Autoridad y límites del corte

```text
Implementation inspeccionada   moragaga/atlanticus:main@2e7500a6b8b4d5bbdad26d807abfa57936db99d5
Decisions inspeccionadas      moragaga/atlanticus-decisions:main@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical remoto previo       moragaga/atlanticus-cannonical:main@2e8bbf4780cafc4cea3b18351861aa97a4fb0053
```

El checkpoint canónico remoto aún documenta C4 PLANNED/CURRENT+FACTS. Git C4 y el handoff previo del Project ya registran receptor Delivery **CURRENT-only**; los reemplazos canónicos C4 anteriores no están acreditados como integrados a Git. Este documento actualiza esa distinción sin declarar qualification física adicional. Tests/manual UX son evidencia local del usuario, no CI ni prueba integrada del Golden Path.

## 2. Recorrido actual, owners y gates

| Etapa | Owner actual | Estado demostrable |
|---|---|---|
| Tool Sources/Projections y conexiones nombradas | Tool/Web | CURRENT; B1d qualification local controlada histórica, no Azure. |
| Confirmed Tool Catalog Cn en Blob | `web/tools/catalog` | CURRENT / C1 CLOSED. |
| Discovery, inspect y confirmación humana | `web/tools/discovery-cosmos` | CURRENT / C1 CLOSED; las Tools Cosmos solas no forman automáticamente el catálogo. |
| Tool Catalog UI reusable | `web/tools/catalog-manager` | CURRENT / C1 CLOSED. |
| Alarm editor Rn/Cn, Save/Validate/Publish | Alarm Web/Domain | Source v3 CURRENT; UX-01/UX-02 CLOSED por código/tests y operación manual básica. |
| Familias/Rules/Messages, asignación y guardado | Alarm Web | VERIFIED / CLOSED en navegador **aislado con catálogo de fixture**. No se demostró recuperación tras reinicio ni guardado durable Blob/Cosmos. |
| Source Key `alarm-configuration` única | Domain + Web/jobs | CURRENT / C2 CLOSED. |
| Alarm Source Blob/Projection Cosmos entrada física | Web/Materialization | Adapters/contrato CURRENT, integración física E2E UNVERIFIED. |
| `APPLICATION` común; leases separados y `VOLUMEN_PATH` manual | Procesos Alarm | CURRENT contrato C2; mismo volumen real entre contenedores UNVERIFIED. |
| Qualification Rn/Cn | Materialization | JSON manual CURRENT; C3 productor automático BLOCKED por decisiones/evidencia ausentes. |
| B.2 READY/BLOCKED, pareja Runtime/Delivery exacta | Materialization | CURRENT y tests históricos; BLOCKED nunca reemplaza READY íntegro. |
| WAL adopción → EFFECTIVE | Persistence/Runtime | CURRENT y gate histórico local; Docker independiente UNVERIFIED. |
| Engine CURRENT v1 completo y FACTS v2 durables | Runtime | CURRENT; preservar ambos productos. |
| Receptor Delivery último CURRENT con pin/READY/EFFECTIVE | `processes/alarms-delivery` | C4 CLOSED por código y regresión local anteriores; NO recibe backlog FACTS. |
| Starter genérico propio con runtime y Home mínima | Web | **PLANNED / ÚNICO PRÓXIMO FOCO**; solo host Configuration Manager temporal existe. |
| Live materializer y `AlarmLiveProjection` | Backend Live | PROJECT CONTRACT AGREED, NOT IMPLEMENTED; no deducir de receiver C4. |
| Management Capture/Projection y History/Analytics | Frentes independientes | PLANNED, fuera del Starter inicial. |
| Ejecución/distribución Docker y Azure real | Integración | UNVERIFIED en este hito. |

## 3. Contratos de la cadena congelados

```text
Tool Sources/Projections
  -> confirmed Tool Catalog Cn (Blob)
  -> Alarm Configuration Source Rn + ToolDependencyManifest Cn (v3)
  -> Alarm Projection Cosmos (binding físico por comprobar)
  -> Qualification vigente: intervención humana/JSON controlado
  -> Materialization: BLOCKED diagnóstico o READY pareja exacta
  -> Engine WAL adoption / EFFECTIVE (pin completo)
  -> Engine CURRENT v1 completo/reemplazable
  -> Engine FACTS v2 inmutables (canal distinto, preservado)
  -> Delivery input LAST CURRENT ONLY, pin/EFFECTIVE/READY exactos
  -> [NO IMPLEMENTED] Live materializer + AlarmLiveProjection
  -> [PLANNED] Command Center Web operacional con Home mínima
```

Pin exacto: `source_key + result_id + manifest_sha256 + resolution_key`; Runtime/Delivery no reemplazan configuración por latest READY ni reinterpretan Rn/Cn desde latest Tool Catalog. `INVALID != REMOVED`, `DISABLED != REMOVED` y `TRACE_ONLY != REMOVED`; READY no activa Engine. Web no procesa WAL ni resuelve priority/routing/cause o deactivation capability por su cuenta.

La salida de Runtime `FACTS v2` y su cursor productor **permanecen CURRENT** después de C4; sólo el receptor de Delivery deja de consumir esos lotes. La recepción del último CURRENT no constituye generación de una proyección Live ni prueba de disparo visible en navegador.

## 4. Hito UX y test local — VERIFIED acotado

El usuario aplicó un parche de 20 archivos para UX-02 y comprobó `--check`, `git apply --check`, `git apply`, `git diff --check` y:

```text
Domain Alarms           59 PASS
Alarm Materialization   64 PASS
Alarm Configuration Web 123 PASS
TOTAL                  246 PASS
```

La versión Git `2e7500a...` contiene UX-02: Rule/Message ofrecen `1..11` o `END_OF_SHIFT`, propagado como límite estático. UX-01 ya implementó cierre de modal sólo tras Save Draft exitoso.

**VERIFIED por test manual posterior:** el usuario abrió el host real con lanzador de prueba completamente aislado, creó familias, Rules y Messages, asignó Messages y guardó. El test NO arrancó Engine/Delivery ni conectó Blob/Cosmos; utilizó una Tool Process sintética y archivos de prueba aislados. **Hallazgos OPEN:** campos ocasionalmente borrados, validaciones que desaparecen, alertas que no se despejan.

**Separación crítica:** la opción authored `END_OF_SHIFT` **no** produce automáticamente un `effective_until`. El Web operacional deberá obtener la hora final real del proceso/turno y remitir UTC; Core no posee calendario de turnos.

## 5. Gate real aún no demostrado y frontera siguiente

No se ha mostrado un único gate que reúna: Tool real + catálogo confirmado + Alarm Source/Projection durable + qualification real/manual explícita + READY + EFFECTIVE + Engine CURRENT + Delivery CURRENT-only + lectura operacional de una proyección Live por la Home. No simular ese gate con fixture UI ni extender el receiver para llamarlo Live sin contrato.

**PLANNED / único foco próximo:** debate/diseño de **Starter Web genérico de Command Center** con composición del runtime y **Home mínima**. Auditar packages, entrypoints, configuración/env, bootstrap, distribución y contratos backend ya existentes. Determinar qué puede comprobarse como gate de **ejecución de alarmas** hasta CURRENT/Delivery y qué necesitaría el futuro Live para mostrarlas en Home. Sin Navigation, Users, Profiles, dashboard complejo, History/Analytics ni cambios de backend no acordados.

Los gates C3 (producer qualification), C5 (evidence), Docker/montaje, Source Blob↔Cosmos real y Live/Management continúan separados o **BLOCKED** por sus contratos. No ampliar el siguiente incremento para cerrar artificialmente todo el Golden Path.
