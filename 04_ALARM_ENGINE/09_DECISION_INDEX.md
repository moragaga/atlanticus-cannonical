# Alarm Engine — Decision Index

Estado: **CURRENT / HISTORICAL SOURCES + IMPLEMENTATION REFINEMENTS / 2026-09-26**

| ID | Tema | Estado |
|---|---|---|
| ALARM-DEF-B1 | Inventario B.1 de Alarm Definition | HISTORICAL / FROZEN INPUT |
| ALARM-PROJ-B2 | Decisiones históricas de proyección/publicación B.2 | HISTORICAL / REFINED |
| ALARM-B2-PURE-RESOLVER | Resolver determinista puro | CURRENT / IMPLEMENTED |
| ALARM-TOOL-MANIFEST | Evidencia Tool exacta congelada con Alarm Source | CURRENT / IMPLEMENTED |
| ALARM-SOURCE-V3 | `AlarmConfigurationSnapshot` + `ToolDependencyManifest` | CURRENT / IMPLEMENTED |
| ALARM-TOOLS-FREEZE | Correlación Cn en validate/publish | CURRENT / IMPLEMENTED |
| ALARM-CONFIG-PROJECTION | Codec, builder, stores Local/Cosmos y composición | CURRENT / IMPLEMENTED; operacional E2E UNVERIFIED |
| ALARM-ROUTING-STRICT | Process -> Integrated Operations -> Strategic, sólo próximo nivel | CURRENT / DECIDED / IMPLEMENTED |
| ALARM-ROUTING-UX | Mismas opciones basadas en policy y Strategic sólo routing | CURRENT / IMPLEMENTED |
| ALARM-RANK-SUPPRESSION | `priority_order` suppression | CURRENT |
| ALARM-DEACTIVATION-CASCADE | Deactivation/cascade vigente | CURRENT |
| ALARM-RUNTIME-VISIBILITY | Sin `delivery_enabled` ni `SHADOW` | CURRENT |
| ALARM-MATERIALIZATION-JOB | Lectura de proyección, qualification, outputs y descarga | PLANNED / NEXT; contratos físicos OPEN |
| ALARM-VISUAL-ROUTING-OWNERSHIP | Independencia conceptual vs sincronización automática del editor | CONFLICT / REQUIERE DECISIÓN EXPRESA |

## Sources

```text
Implementation: moragaga/atlanticus@411aea44ac60c09d2b07ce41d34c3f378788b97b
Historical: moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical input: moragaga/atlanticus-cannonical@83cd871c8418e37d2c29dff30e2ea5ef54bda4a0
```

Decisiones históricas preservadas: separación Live/Management; una resolución coherente produce Runtime + Delivery de la misma key; `INVALID != REMOVED`; `READY != EFFECTIVE`; backend es autoridad de prioridad/routing.

## Reemplazos/refinamientos CURRENT

**Autoridad física:** SharePoint como autoridad general queda HISTORICAL donde Blob/Storage ya es la fuente durable. No alterar dominios no migrados.

**Topología Tool:** `Confirmed Tool Catalog -> Command Center Cosmos` es **SUPERSEDED**. CURRENT: upstream Tool projections + Storage prior state -> reconciliation/certification -> Confirmed Tool Catalog -> Storage -> END.

**Exact Alarm/Tool correlation:** re-resolver la misma release Alarm R1 con Tool Catalog C2 posterior es **SUPERSEDED**. CURRENT: cada Rn congela su `ToolDependencyManifest(Cn)`; sólo nueva publicación Alarm puede adoptar nueva Tool evidence.

**Save gate:** `LATEST SAVED = LATEST VALID_AT_SAVE`, pero `VALID_AT_SAVE != READY != EFFECTIVE`. La publicación actual valida agregado y correlación Tool; evaluator/Tool GREEN y routing completo pertenecen a B.2.

**Strict routing:** la deliberación anterior a este hito contempló mismo nivel como posible excepción y salto directo Process -> Strategic. **SUPERSEDED** ambas alternativas por decisión explícita del usuario. Queda **FROZEN** la ruta exclusiva `PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC`, sin saltos/retornos/mismo nivel; cero destinos en C1/C2 válido, sin degradar criticidad a C3; Strategic terminal y sin contrato visual.

**Temporización:** C1 inmediato; C2 waits positivos acumulados entre pasos **habilitados** y deadlines desde el inicio de la ocurrencia; C3 origen únicamente. No reinterpretar 20+20 como segundos 20 ni como 20+40 de espera incremental.

**Visual:** el acuerdo de presentación deduce estrategias de Tool kind (Process CAROUSEL / Integrated Operations QUEUE_IN_QUEUE), sin scheduler ni campos nuevos. La sincronización automática de visual targets desde routing existe en código, pero contradice la afirmación canónica de independencia; mantener CONFLICT visible, no inventar resolución.

## Próximo foco único

Job/proceso backend de B.2 Materialization para adquirir la proyección operacional exacta, qualification y persistir/dejar disponibles Runtime/Delivery/findings. Runtime Adoption, Live Delivery y Analytics siguen después, en incrementos separados.
