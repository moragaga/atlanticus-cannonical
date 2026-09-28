# Alarm Engine — Decision Index

Estado: **CURRENT — inventario de contratos, refinamientos y conflictos; NO crea decisiones en** `atlanticus-decisions`. Corte 2026-09-28. Código del hito contrastado remotamente en `atlanticus@c67fcb5b105cc561c16719a8bca4ea5aa74c3fae`; main actual `bc1d73742bcb04eb495bbbb1725a8ad23d4eff38` contiene un cambio posterior ajeno a alarmas; decisions `50c2bb3...`; canonical base `5558cf9...`. Los identificadores de la tabla son descriptivos, no IDs de nuevas decisiones.

| Frontera | Estado y evidencia |
|---|---|
| B.1 AlarmDefinition/Rule/Occurrence/Episode | CURRENT en Domain/Core; conflictos de ciertas mutaciones B1 vs ejecutor siguen OPEN. |
| Source v3 + Tool manifest exacto Rn/Cn | CURRENT; source v2 SUPERSEDED, sin decoder legacy acordado. |
| Strict routing PROCESS→INTEGRATED_OPERATIONS→STRATEGIC→END | CURRENT; congelado para este frente. |
| Materialization B.2 + qualification | CURRENT: resolver puro y publicación local inmutable READY/BLOCKED; antigua salida Cosmos SUPERSEDED, Cosmos puede seguir como entrada. |
| B1 exact artifact pin | CURRENT: source/result/hash/resolution, no sólo Rn/Cn. |
| B2a WAL adoption | CURRENT: V1 sin grupos y V2 con grupos; **ambas vigentes**, no considerar V1 legacy. |
| B2b EFFECTIVE recuperable | CURRENT: autoridad WAL, `effective-head.json` proyección. |
| B2c operación y ejemplo | CURRENT: composición ejecutable y ejemplo aislado; no registrar ejemplo como evaluación productiva. |
| **B2c.7a** publicación CURRENT/FACTS | CLOSED gate local; CURRENT v1 y FACTS **refinados a v2 por B2c.7d**. |
| **B2c.7b** receptor independiente | CLOSED gate local; receptor strict v2 tras refinamiento d, cursor propio, sin WAL. |
| **B2c.7c** integración | CLOSED gate local con Engine real + datos controlados + reinicio por nuevas instancias. |
| **B2c.7d** continuidad FACTS | CLOSED gate local; contrato FACTS v2 encadenado, fallos fail-closed. |
| Distribución + ejecución Docker independiente | PLANNED; foco único siguiente. |
| AlarmLiveProjection / Management Capture / History | PLANNED/SEPARATE, no considerar implementados por existir input Delivery. |

## Genealogía de decisiones refinadas y elementos SUPERSEDED

1. Source v2 y salida B.2 Cosmos anterior: SUPERSEDED. La pareja Runtime/Delivery READY local es inmutable y exacta; Source v3 congela ToolDependencyManifest.
2. READY y Rn/Cn por sí solos **nunca** constituyen EFFECTIVE; la autoridad es adopción durable en el WAL, con pin source/result/manifest/Rn-Cn. `effective-head.json` es proyección.
3. La descripción histórica de B2c como sólo PLANNED o del job Runtime sin wiring operativo es SUPERSEDED **en las piezas implementadas**; ello no prueba entorno real desplegado.
4. El registro automático del ejemplo `mina.threshold` es SUPERSEDED; el catálogo productivo permanece vacío mientras no se autoricen nuevas lógicas. La API existente de requisitos estáticos/dinámicos se conserva.
5. La propuesta inicial de FACTS v1 sin cadena fue **SUPERSEDED como formato operativo** por FACTS v2 encadenado. No confundir esto con WAL adoption V1/V2: son contratos diferentes; **no retirar** el WAL adoption V1.
6. La aproximación «un checksum en cada lote y comprobar sólo el último cursor es continuidad histórica suficiente» quedó refinada: v2 enlaza predecesores y el receptor audita huecos/cadena de recibidos. No implica firma/autenticidad ni conservación infinita.
7. Una suposición de que Delivery input equivale a Live Delivery completo queda SUPERSEDED: el job recibido sólo verifica y persiste entradas; no materializa todavía `AlarmLiveProjection` ni hechos propios de entregas efectivas.

## CONFLICT / OPEN sin conciliación silenciosa

- B.1 frozen vs `adoption.py`: decisiones históricas aspiran a permitir compatibilidad de `evaluator_key`/`kind` y migraciones de `priority_group`; código inspeccionado rechaza mutaciones. Requiere decisión aparte; no cambiar aquí.
- B.1 Special Cascade histórico vs suppression uniformemente por ranking vigente en `04_ALARM_ENGINE/06_MANAGEMENT.md` y código Core. El contrato histórico y el estado CURRENT deben contrastarse antes de marcar una regla como refinamiento formal en decisions.
- Message inactive: intención Project indica Message existente pero no seleccionable para acciones nuevas; hay formulación histórica de B.1/B.2 que debe aclararse formalmente en decisions. No modificar comportamiento durante el cierre.
- Visual targets vs routing: separación conceptual Live acordada frente a sincronización/autoría Web previa; no asumir correlación automática.
- Deactivation hasta fin de turno: Domain/Web aún representan límite numérico 1..12 horas; necesidad declarada no trae algoritmo/contrato aprobado. OPEN separado.
- Python objetivo 3.14.7 del Project vs `requires-python ==3.14.2` en Command Center: OPEN transversal.
- FACTS v1→v2: contrato runtime v2 estricto, archivos/volúmenes existentes pueden ser v1; falta inventario y resolución explícita de migración. **No añadir legacy por inferencia**.

Los conflictos con decisiones históricas se registran, **no** se resuelven mediante este reemplazo documental. No se escribieron decisiones nuevas en Git.
