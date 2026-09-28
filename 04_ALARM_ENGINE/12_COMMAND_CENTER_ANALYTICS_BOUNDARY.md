# Alarm Engine — Command Center Analytics Boundary

Estado: **CANDIDATE para read models Analytics; CURRENT para emisión Engine FACTS v2 y recepción independiente Delivery**. Corte: 2026-09-28. No elevar por inferencia una propuesta de proyección histórica a contrato implementado.

## Responsabilidades presentes

Alarm Engine pertenece al backend ADA Command Center; no contiene lógica de dashboard. Confirma hechos en su WAL y B2c.7a/d publica copias FACTS v2 durables, inmutables, identificadas por commit y encadenadas por `previous_batch`. B2c.7b/d habilita un job de recepción Delivery que guarda lotes y cursor propio; no procesa aún Analytics. La prueba B2c.7c verificó el intercambio controlado y reinicio por nuevas instancias.

```text
Engine WAL (única autoridad de commits)
     |
     +--> Engine CURRENT v1 ----> Delivery input ------> [PLANNED] Live Projection
     |
     +--> Engine FACTS v2 -----> Delivery input ------> [CANDIDATE] History/Analytics read model
```

**No confundir** la presencia de un archivo FACTS con una base histórica de consultas implementada. La recepción no equivale a retención externa indefinida, construcción de índices ni proyección para Web.

## Fuentes históricas candidatas, ya transportables si existen en commit

- Occurrence/Episode y sus transiciones;
- Journey y Evidence con muestreo según políticas existentes;
- gestión, deactivation e input receipts;
- routing/assignment y cambios de prioridad registrados;
- referencia exacta de configuración/herramientas por artefacto.

No almacenar muestreos repetitivos INACTIVE sólo para fabricar una serie histórica; evidence es de eventos y sampling conforme a Engine. Un futuro histórico puede solicitar datos físicos de mayor resolución a su fuente original por otro contrato; no fingir que FACTS los contiene.

## Tres proyecciones conceptualmente independientes

1. **Live:** estado operacional actual enriquecido por Delivery Configuration exacta.
2. **Management:** registro y resultado de acciones capturadas con su propio ciclo de vida.
3. **History/Analytics:** read model de evolución basado en hechos durables y hechos futuros de entrega.

La Web no lee WAL, no ordena prioridad y no reinterpreta Rule/cause por su cuenta. Analytics no modifica Engine ni su cursor de exportación. Delivery debe conservar su propio avance. Los tiempos reales de despacho/escalamiento son **hechos futuros de Delivery**, no se inventan en los lotes que produce Engine.

## Límite actual y foco posterior

Verificación local de continuidad FACTS v2: 32 específicas y 162 PASS/1 SKIPPED de suite conjunta comunicadas; **no** implica Docker ni almacenamiento histórico operando. El foco único siguiente acordado es distribución y Docker independiente de Engine/Delivery. Live, Capture y History continúan separados; no abrirlos en el mismo incremento.
