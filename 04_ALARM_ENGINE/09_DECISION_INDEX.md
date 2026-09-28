# Alarm Engine — Decision Index

Estado: **CURRENT / BASE B.1 Y B.2 + ADOPCIÓN DURABLE B2a + EFFECTIVE B2b IMPLEMENTADOS; EJECUTOR COMPLETO B2c PLANNED**.

Fuentes leídas en Git READ ONLY (2026-09-27): `atlanticus@ebc7a8bf8d49e931fd4e2487dac5ee036011a0a5`, `atlanticus-cannonical@be2c424c44648e6488daae36d410cf425eed02b8`, `atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`. Las etiquetas `ALARM-*` siguientes son **inventario documental descriptivo**, no identificadores oficiales nuevos de decisiones en `atlanticus-decisions`.

| Identificador descriptivo | Frontera / decisión | Estado demostrado en este corte |
|---|---|---|
| ALARM-DEF-B1 | Rules, Messages y semántica deseada | HISTORICAL FROZEN, con conflictos de política de cambios aún OPEN. |
| ALARM-PROJ-B2 | Proyección/materialización | HISTORICAL REFINED: salida Cosmos anterior SUPERSEDED por volumen local. |
| ALARM-SOURCE-V3 | `AlarmConfigurationSnapshot` schema 3 y manifest Tool Cn en Rn | CURRENT; source v2 SUPERSEDED, sin decoder legacy. |
| ALARM-TOOL-FREEZE | Manifest exacto, drift Validate/Publish | CURRENT; no reinterpretar Rn con latest. |
| ALARM-CONFIG-PROJECTION | Builder, codec, proyección Local/Cosmos entrada | CURRENT; infra E2E UNVERIFIED. |
| ALARM-B2-RESOLVER | `resolve_alarm_configuration` puro | CURRENT; el paquete B.2 completo también contiene I/O. |
| ALARM-ROUTING-STRICT | PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC | CURRENT / FROZEN; sin same-tier ni saltos. |
| ALARM-MATERIALIZATION-EXECUTABLE | Job Cosmos entrada + qualification controlada | CURRENT; gate local CLOSED, productores operativos UNVERIFIED. |
| ALARM-MATERIALIZATION-COSMOS-OUTPUT | Salida B.2 en Cosmos | SUPERSEDED / eliminada; no dual-write. |
| ALARM-MATERIALIZATION-LOCAL-OUTPUT | READY/BLOCKED, versiones inmutables, manifest/hash | CURRENT / CLOSED local; volumen físico UNVERIFIED. |
| ALARM-LOCAL-READER | READY y versión exacta compartida | CURRENT. |
| ALARM-EXACT-ARTIFACT-B1 | `AlarmConfigurationArtifactRef(source,result_id,manifest_sha256,key)` | CURRENT. |
| ALARM-ADOPTION-PLAN-B1 | Unión de identidades definidas, `ADDED`/`ENABLED` | CURRENT; planificación no equivale a ejecución. |
| ALARM-ADOPTION-WAL-V1 | Adopción global cero grupos y pin exacto | CURRENT / B2a.1 CLOSED local. |
| ALARM-ADOPTION-WAL-V2 | Adopción global 1..N grupos con referencias/hash | CURRENT / B2a.2 CLOSED local. |
| ALARM-EFFECTIVE-PROJECTION | Effective Head exacto reconstruible y validado | CURRENT / B2b.1 CLOSED local. |
| ALARM-RUNTIME-EFFECTIVE-READER | Lector Runtime exacto con revalidación de selección | CURRENT / B2b.2 CLOSED local. |
| ALARM-ADOPTION-EXECUTION | Aplicar integralmente B1 y confirmar V1/V2 | CURRENT / PARCIAL en ejecutor antiguo; B2c PLANNED. |
| ALARM-QUALIFICATION-PRODUCERS | Evaluadores/productores GREEN reales | OPEN / UNVERIFIED, JSON controlado no es productor real. |
| ALARM-DELIVERY-LIVE-MGMT | Delivery/Live, Capture y Projection | PLANNED / SEPARATE. |
| ALARM-VISUAL-ROUTING-OWNERSHIP | Targets visuales conceptuales vs editor acoplado | OPEN / CONFLICT separado. |

## SUPERSEDED / REFINED en genealogía

1. **SUPERSEDED — salida Materialization Cosmos:** mantiene Cosmos sólo como entrada de proyección; versión de salida local inmutable, sin store de salida Cosmos ni codec privado duplicado.
2. **SUPERSEDED — source v2 y viejas variantes de strict routing:** contrato CURRENT de Source es schema 3 con manifest Tool exacto; routing sólo nivel siguiente. No introducir decoders o adaptadores temporales por comodidad.
3. **REFINED — identidad Rn/Cn insuficiente:** B1 añadió `source_key`, `result_id` y SHA256 manifest como pin exacto. Rn/Cn no desaparece: identifica la resolución lógica, pero no diferencia evidencia de qualification con mismo Rn/Cn.
4. **REFINED — universo de planning:** B1 considera source definido UNION target definido, incluso Rules disabled, y añade `ADDED`/`ENABLED`; el ejecutor sigue por debajo de ese contrato.
5. **REFINED / IMPLEMENTED — adopción durable global:** lo antes PLANNED ya está físicamente representado en WAL existente mediante V1 (cero grupos) y V2 (1..N grupos con hash), sin grupo sintético ni segundo journal. Los commits ordinarios mantienen su contrato.
6. **REFINED / IMPLEMENTED — Effective Head:** modelo `alarm-effective-head.v1` con `adoption_id`, hash, posición WAL, `target_artifact_ref` y `effective_at`; proyección publicada tras materialized, reparable desde WAL. No hay `materialization/effective.json`.
7. **REFINED / IMPLEMENTED — Runtime exacto:** lector B2b.2 obtiene EFFECTIVE de Persistence, lee versión exacta y detecta cambios posteriores. **No** enlaza automáticamente el ejecutor/job existente.
8. **FROZEN — READY distinto de EFFECTIVE:** incluso un cambio Delivery-only puede requerir adopción global cero grupos; publicar READY, planificar o leer READY no son adopciones operacionales.

## Conflictos actuales que requieren decisión expresa, no resolución silenciosa

- **DECISIONS B.1 vs MAIN:** `evaluator_key` y `kind` son COMPATIBLE deseado y cambio de `priority_group` exige reconciliar ambos grupos; `adoption.py` actualmente rechaza esas mutaciones. B2c no debe resolverlas incidentalmente ni describirlas como disponibles.
- **B1 planner vs MAIN executor:** `plan.is_adoptable=True` no implica `plan.requires_execution_upgrade=False`; `adoption_execution.py` exige `source.plan_for(identity)` para todo cambio distinto de `UNCHANGED`, por lo que falla en `ADDED`, `ENABLED` y `REMOVED` desde source disabled. También confirma grupos mediante `commit_batch` sin adoptar configuración V1/V2.
- **MAIN vs canonical existente:** al corte `be2c424...`, `00`, `02`, `05`, `07`, `08`, `09`, `10`, `11` y `13` aún tratan adopción durable/Effective Head como PLANNED o sólo hacen referencia a B1. Estos reemplazos proponen corregir ese desfase, no afirman integración remota.
- **Python baseline:** Project fija 3.14.7; metadata de los paquetes Command Center de este corte exige `==3.14.2`. Frente separado; no cambiar metadatos en este cierre.
- **Histórico de Special Condition y visual routing:** contrastar definiciones con contratos Core/editor actuales en su foco; no inferir que B2a/B2b resolvieron esas diferencias.

Git permanece READ ONLY desde el asistente; ninguna fila crea una decisión formal nueva en `atlanticus-decisions` ni certifica infraestructura productiva.
