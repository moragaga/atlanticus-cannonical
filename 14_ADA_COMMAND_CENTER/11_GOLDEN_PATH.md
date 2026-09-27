# ADA Command Center — Golden Path

Estado: **PARTIALLY IMPLEMENTED / MATERIALIZATION LOCAL OUTPUT NEXT**

Corte: `atlanticus@b600ca591b56d0924aed752dfae6e9fab2c6f1d6`; canonical `772d15078c97802d58d8b658b0d5d5b928fa2ed5`; decisions `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`.

## Flujo objetivo con estados reales

| Paso | Responsable | Estado de este corte |
|---|---|---|
| Tool owners publican proyecciones; Command Center reconcilia y confirma Cn | Tool / Manager | CURRENT. |
| Confirmed Tool Catalog Cn -> Storage, sin proyección consolidada de vuelta a Cosmos | Tool | CURRENT. |
| Alarm authoring pin Cn, Validate/Publish con drift check | Alarm Manager | CURRENT. |
| Source release Rn congela `ToolDependencyManifest(Cn)` schema v3 | Alarm Configuration | CURRENT. |
| Builder/codec/adapters Local/Cosmos para `ProjectionRecord[AlarmConfigurationSnapshot]` | Web persistence | CURRENT; operacional real UNVERIFIED. |
| Materialization adquiere proyección activa, fija release/evidencia y ejecuta B.2 | Backend process | CURRENT EN CÓDIGO v0.2.1; E2E UNVERIFIED. |
| Materialization publica salida Cosmos monolítica | Backend process | CURRENT EN CÓDIGO / SUPERSEDED POR DECISIÓN. |
| Materialization publica artefactos READY coherentes en volumen | Backend process | DECIDED / PLANNED. |
| Runtime Adoption carga versión local EXACTA y confirma EFFECTIVE | Alarm Runtime | PLANNED / helpers parciales CURRENT. |
| Delivery carga su par local de la misma key efectiva y combina Engine current state | Alarm Live Delivery | PLANNED / SEPARATE. |
| Web consume Live Projection y Management en sus respectivos canales | Web | PLANNED / SEPARATE. |

## Separaciones obligatorias

```text
Rn/Cn -> snapshot congelado
VALID_AT_SAVE != READY != EFFECTIVE
READY -> Runtime + Delivery misma AlarmResolutionKey
BLOCKED -> findings, sin artefactos ejecutables
INVALID != REMOVED; DISABLED != REMOVED; TRACE_ONLY != REMOVED
```

Si aparece C2 después de publicar `R1/C1`, la release Alarm no pasa a `R1/C2` sin una nueva publicación. El proceso usa la evidencia congelada, no latest Tool Catalog.

**Contrato de desempeño decidido:** Engine y Delivery **no vuelven a Cosmos para leer su configuración**; usan el volumen local por versión exacta durante su ciclo de vida. El medio físico para futura Live Projection Web no queda decidido aquí.

## Único siguiente entregable

Sustituir la salida Cosmos del proceso 0.2.1 por salida local versionada/coherente comprobable, con manifest/integridad, idempotencia y BLOCKED seguro. Corregir y ejecutar tests/revisión de formato en entorno real de desarrollo **sin Cosmos real**. Dejar integración de infraestructura, productores automáticos, Runtime y Delivery para hitos separados.
