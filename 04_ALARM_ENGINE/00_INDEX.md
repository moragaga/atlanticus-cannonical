# Alarm Engine — Index

Estado: **CURRENT — B2c.7a/b/c/d implementados en el alcance comprobado; qualification de distribución y Docker PLANNED**. Corte documental: 2026-09-28. Reemplazo local pendiente de incorporación a canonical.

```text
HEAD del hito verificado   atlanticus@c67fcb5b105cc561c16719a8bca4ea5aa74c3fae
HEAD remoto main leído      atlanticus@bc1d73742bcb04eb495bbbb1725a8ad23d4eff38 (commit adicional sólo ADA Generic)
Decisions remoto leído       atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical de partida         atlanticus-cannonical@5558cf9d92d9b21758500024b6099011416d78da
```

El SHA del hito fue corroborado directamente en Git; las 8 rutas B2c.7d fueron verificadas en el commit. No asumir CI por esa lectura. Los resultados de `pytest`/Ruff proceden de logs de su entorno Python 3.14.2. Este índice describe **únicamente** la frontera Alarm Engine / Delivery.

| Archivo | Contenido y condición |
|---|---|
| `01_DOMAIN_MODEL.md` | Core/Domain: identidad, contratos y límites; conservado sin cambio semántico en B2c.7. |
| `02_RUNTIME_AND_LIFECYCLE.md` | Lifecycle, gestión, prioridad y estado por grupo; no modificado por la publicación. |
| `03_PERSISTENCE_AND_RECOVERY.md` | WAL autoritativo, Durable/Materialized Head y EFFECTIVE; sigue siendo único journal del Engine. |
| `04_CONCURRENCY_LEASES_AND_FENCING.md` | Autoridad de lease y fenced mutations; no sustituir por escritura libre. |
| `05_PROJECTION_AND_PUBLICATION.md` | **Actualizado:** CURRENT v1, FACTS v2 encadenados, cursores separados, recepción Delivery. |
| `06_MANAGEMENT.md` | Gestión, deactivation y suppression existente; no ampliar desde B2c.7. |
| `07_CONFIGURATION_AND_MATERIALIZATION.md` | Source v3, qualification y READY/BLOCKED exacto. |
| `08_QUALIFICATION_BASELINE.md` | **Actualizado:** evidencias locales B2c.7a/b/c/d y límites. |
| `09_DECISION_INDEX.md` | **Actualizado:** refinamientos, superseded y conflictos abiertos. |
| `10_OPEN_ITEMS.md` | **Actualizado:** v1→v2, distribución, Docker, límites posteriores. |
| `11_SOURCE_LEDGER.md` | **Actualizado:** commits, fuentes reales, artefactos y resultados. |
| `12_COMMAND_CENTER_ANALYTICS_BOUNDARY.md` | **Actualizado:** FACTS publicado no equivale a History/Analytics implementado. |
| `13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md` | **Actualizado:** composición, Engine outputs e integración Delivery. |
| `../14_ADA_COMMAND_CENTER/16_ALARM_LIVE_DELIVERY_CONTRACT.md` | **Actualizado:** progreso del productor/consumidor, contrato conceptual Live todavía no materializado. |

## Secuencia CURRENT hasta este cierre

```text
Source v3 (Rn + ToolDependencyManifest Cn)
  -> Materialization READY exacto: Runtime/Delivery; o BLOCKED
  -> B1 plan de adopción por artefacto exacto
  -> B2a WAL V1 (0 grupos) / V2 (1..N grupos)
  -> B2b EFFECTIVE verificable, derivado del WAL
  -> B2c Engine Runtime ejecutable con sesión/adopción fijadas
  -> B2c.7a Engine publica CURRENT v1 + FACTS (ahora v2)
  -> B2c.7b job independiente Delivery recibe y conserva cursor propio
  -> B2c.7c integración controlada y reinicio de componentes
  -> B2c.7d cadena FACTS v2 + verificación de huecos/recovery
  -> [PLANNED] qualification distribución + Docker independiente
  -> [SEPARATE] AlarmLiveProjection / Capture / History
```

La prueba B2c.7c usa Engine real, datos controlados y recreación de instancias; **no** acredita procesos Docker independientes, despliegue Azure, almacenamiento multi-host ni datasets productivos. No tratar un JSON Schema productivo como fixture: `engine_resolved_current_state.v1.schema.json` y `engine_committed_facts_batch.v2.schema.json` son contratos estáticos; `current/latest.json`, `facts/facts-*.json` y cursores son datos generados.

**Foco único siguiente:** qualification/distribución y ejecución Docker independiente Engine/Delivery, respetando los contratos existentes y sin ampliar código de producto antes de auditar artefactos y entorno.
