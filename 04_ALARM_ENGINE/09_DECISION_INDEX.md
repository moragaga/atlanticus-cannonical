# Alarm Engine — Decision Index

Estado: **CURRENT / MATERIALIZATION LOCAL Y B1 CLOSED LOCALMENTE / GLOBAL ADOPTION PLANNED (2026-09-27)**

Fuentes verificadas: implementación `atlanticus@c8f23d91ae1cb817be55b4b812b22ffca518880e`; baseline canonical previa `58241ddb6db5adbd2e783c7ec9f456f1bda5a321`; genealogy decisions `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`.

| ID descriptivo | Decisión/frontera | Estado comprobado |
|---|---|---|
| ALARM-DEF-B1 | Definición B.1, Rules, Messages y semántica deseada | HISTORICAL FROZEN, con discrepancias expresas en políticas de cambio. |
| ALARM-PROJ-B2 | Proyección y materialización | HISTORICAL REFINED: salida B.2 Cosmos sustituida por volumen local. |
| ALARM-SOURCE-V3 | `AlarmConfigurationSnapshot` schema 3 y manifest Tool exacto | CURRENT / IMPLEMENTED; v2 SUPERSEDED sin decoder legacy. |
| ALARM-TOOL-FREEZE | Cn en Rn, drift Validate/Publish | CURRENT / IMPLEMENTED; no consultar latest al ejecutar B.2. |
| ALARM-CONFIG-PROJECTION | Builder, codec y stores Local/Cosmos | CURRENT / IMPLEMENTED; operación Cosmos E2E UNVERIFIED. |
| ALARM-B2-RESOLVER | `resolve_alarm_configuration` puro | CURRENT / IMPLEMENTED; **paquete** B.2 contiene ahora I/O de lectura compartido. |
| ALARM-ROUTING-STRICT | Exclusivamente Process -> Integrated Operations -> Strategic | CURRENT / FROZEN. |
| ALARM-MATERIALIZATION-EXECUTABLE | Job sobre adquisición Cosmos, qualification y B.2 | CURRENT / IMPLEMENTED, gate unitario local CLOSED. |
| ALARM-MATERIALIZATION-COSMOS-OUTPUT | Resultado B.2 de salida en Cosmos | SUPERSEDED / RETIRADO del código actual. |
| ALARM-MATERIALIZATION-LOCAL-OUTPUT | Versiones locales inmutables, manifest/hash, READY/BLOCKED | CURRENT / IMPLEMENTED, gate local CLOSED, validación física multi-host UNVERIFIED. |
| ALARM-LOCAL-READER | Lector exacto compartido y adapter de Runtime | CURRENT / IMPLEMENTED, gate local CLOSED. |
| ALARM-EXACT-ARTIFACT-B1 | `AlarmConfigurationArtifactRef(source_key,result_id,manifest_sha256,resolution_key)` | CURRENT / IMPLEMENTED. |
| ALARM-ADOPTION-PLAN-B1 | Universo definido de origen UNION destino, ADDED/ENABLED | CURRENT / IMPLEMENTED, **sólo planificación**. |
| ALARM-ADOPTION-EXECUTION | Ejecutar toda disposición B1 y reconciliar hot state | CURRENT / PARCIAL en código; ampliación PLANNED y sin autorización de implementación. |
| ALARM-EFFECTIVE-GLOBAL | Commit global durable, recovery y Effective Head exacto | PLANNED; contrato físico todavía no aprobado/implementado. |
| ALARM-QUALIFICATION-PRODUCERS | Productores GREEN/evaluator reales | OPEN / UNVERIFIED; proveedor JSON es manual/controlado. |
| ALARM-VISUAL-ROUTING-OWNERSHIP | Visual independiente conceptualmente vs editor acoplado | CONFLICT / DEBATE SEPARADO. |

## Decisiones reemplazadas/refinadas

1. **SUPERSEDED:** publicación de salida B.2 en Cosmos y consumo posterior de esa salida desde Cosmos. CURRENT: Materialization sigue leyendo **entrada** Cosmos y escribe artefactos locales de salida. No hay dual-write ni adapter Cosmos de salida heredado.
2. **SUPERSEDED:** codec Runtime/Delivery privado/duplicado en el proceso. CURRENT: codec y lector están en `backend/alarms/materialization`; el resolver B.2 permanece puro, **no** el paquete entero.
3. **REFINED:** identificar una adopción únicamente con `AlarmResolutionKey(Rn,Cn)` no basta para elegir una materialización. B1 incluye `result_id` y SHA256 de manifest más `source_key`, sin modificar el significado de Rn/Cn.
4. **REFINED:** el plan anterior consideraba sólo alarmas ejecutables de origen. B1 planifica `source.defined_alarm_identities UNION target.defined_alarm_identities` y añade `ADDED`/`ENABLED`. Eso **no** implementa su ejecución.
5. **FROZEN:** READY y EFFECTIVE son estados diferentes; una materialización READY o plan B1 no es adopción operativa. Un cambio sólo de Delivery también puede requerir nueva adopción global aunque no altere hot state.
6. **REFINED (objetivo, no código):** el contrato conceptual futuro de `ConfigurationAdoptionCommit`/Effective Head debe referirse al artefacto exacto, además de Rn/Cn. Los campos definitivos, ubicación, secuencia de publicación y recovery se decidirán sobre WAL existente en otro incremento; no documentar formato propuesto como CURRENT.
7. **FROZEN:** strict routing reemplaza variantes históricas same-tier/saltos. La independencia conceptual de visual targets frente al comportamiento del editor continúa en conflicto y fuera del alcance.

## Conflictos no resueltos — no reinterpretar

- **DECISIONS B.1 vs CURRENT Runtime:** cambios de `evaluator_key`/`kind` tienen semántica deseada COMPATIBLE y cambios de `priority_group` migración estructural de dos grupos; el planificador actual los rechaza. Es una evolución pendiente, no una incompatibilidad a parchear incidentalmente.
- **CURRENT B1 planner vs CURRENT executor:** `is_adoptable` puede ser `True` para ADDED/ENABLED mientras `requires_execution_upgrade` también es `True`; `adoption_execution.py` pre-B1 sigue exigiendo un source plan ejecutable para todo cambio no UNCHANGED. No invocar como si ejecutara esas transiciones.
- **CANONICAL previo vs MAIN:** documentos del baseline `58241d...` declaraban publicación local y lectura B1 como pendientes. Este reemplazo actualiza esa diferencia conforme al commit actual; aún requiere integración humana en canonical.
- **Target de Python vs metadatos:** Project base `3.14.7` y paquetes de este corte fijan `==3.14.2`. Diferencia transversal separada.

Esta tabla representa estados auditados, no crea identificadores formales nuevos de decisiones en `atlanticus-decisions`: las etiquetas `ALARM-*` sirven para inventario documental. No escribir en Git durante el cierre.
