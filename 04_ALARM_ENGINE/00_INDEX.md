# Alarm Engine — Index

Estado: **CURRENT — B2c.5c y B2c.5d integrados en código; B2c.6 PLANNED**. Corte: 2026-09-28.

```text
Implementación     moragaga/atlanticus@a799dc15105d3e037f36ab77129ef0cfa8999013
Decisiones         moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical de base  moragaga/atlanticus-cannonical@46877f174513b2475f17b7dc739cd43951fa4ed0
```

Este archivo describe el estado del frente **Alarm Engine**, no certifica otros frentes del monorepo. Los reemplazos son locales hasta que el usuario los integre; volver a verificar HEAD y diff antes de copiar.

| Archivo | Propósito / situación |
|---|---|
| `01_DOMAIN_MODEL.md` | Identidad y Core; contratos existentes, no reescritos en B2c.5d. |
| `02_RUNTIME_AND_LIFECYCLE.md` | Reductor, lifecycle, prioridad y coordinación con adopción. |
| `03_PERSISTENCE_AND_RECOVERY.md` | WAL, confirmación V1/V2, Durable/Materialized y EFFECTIVE. |
| `04_CONCURRENCY_LEASES_AND_FENCING.md` | Leases/fencing, conservados. |
| `05_PROJECTION_AND_PUBLICATION.md` | READY, EFFECTIVE y futuras proyecciones; READY no es autoridad operativa. |
| `06_MANAGEMENT.md` | Gestión/desactivación; fin de turno permanece abierto en configuración Web/Domain. |
| `07_CONFIGURATION_AND_MATERIALIZATION.md` | Source v3, B.2 y frontera con la configuración ejecutable. |
| `08_QUALIFICATION_BASELINE.md` | Evidencia histórica y nuevas suites locales B2c.5c/B2c.5d. |
| `09_DECISION_INDEX.md` | Genealogía, refinamientos y conflictos que no se resuelven silenciosamente. |
| `10_OPEN_ITEMS.md` | OPEN restantes y próximo foco único. |
| `11_SOURCE_LEDGER.md` | Trazabilidad por SHA, rutas de código y logs. |
| `12_COMMAND_CENTER_ANALYTICS_BOUNDARY.md` | Frontera Analytics CANDIDATE, no ampliada aquí. |
| `13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md` | B1/B2a/B2b/B2c y catálogo; siguiente frontera de composición. |

## Cadena vigente al corte

```text
Source v3: AlarmConfiguration Rn + ToolDependencyManifest(Cn)
  -> proyección Cosmos de entrada + resolver B.2/qualification
  -> pareja Runtime/Delivery READY local exacta o BLOCKED
  -> B1: revisión y plan de adopción con pin exacto
  -> B2a: WAL durable adopción global V1 (0 grupos) / V2 (1..N)
  -> B2b: Effective Head reconstruible + lectura EFFECTIVE exacta
  -> B2c: job/adopción/sesión fijada + ciclo de evaluación
  -> B2c.5c: fuente/partición actual + requisitos + lectura consolidada
  -> B2c.5d: catálogo de contratos, productivo vacío; ejemplo controlado separado
  -> [PLANNED B2c.6] composición ejecutable real de las dependencias existentes
  -> [SEPARATE] qualification operacional, datasets reales, Live, Management Capture, History
```

**Congelado:** READY != EFFECTIVE; artefacto de revisión exacta por source/result/hash/Rn-Cn; no crear WAL paralelo, grupo sintético, registro automático de ejemplos ni soporte legacy no acordado. Las rutas del proceso y su condición de wheel distribuible no sustituyen la validación de una composición productiva real. No asumir que la suite local prueba Cosmos/Blob ni datos PI reales.
