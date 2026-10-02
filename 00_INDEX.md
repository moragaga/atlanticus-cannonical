# Atlanticus Canonical Context — Index

Estado: **CURRENT — Dual App Durable + Master Projection cerrado; Source convergence NEXT (2026-10-02)**

## Autoridades del cierre

```text
Implementation evidence checkpoint  moragaga/atlanticus@7bd11afdf2af82c56fb100f4aa5336c039d9bd22
Decisions                           moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical pre-replacement           moragaga/atlanticus-cannonical@cedbe3156bc384add0de58cda27f2b77628c133d
```

El SHA de implementación es evidencia del hito, no baseline rígido para cambios paralelos.

## Estado por frente

| Ubicación | Estado relevante |
|---|---|
| `01_CURRENT_STATE.md` | Estado ejecutivo vigente del hito dual y siguiente frontera Source. |
| `02_ARCHITECTURE.md` | Master Projection genérico, product composition, Source ownership y tooling boundaries. |
| `03_DECISIONS_CURRENT.md` | Decisiones congeladas/refinadas de este cierre. |
| `04_ALARM_ENGINE/` | No revalidado ni modificado por este hito. |
| `05_ENGINEERING_BASELINE.md`, `06_OPERATING_MODEL.md` | Baseline general; Python Web 3.14.2 continúa CURRENT. |
| `07_VALIDATION_BASELINE.md` | Evidencia local del durable runtime composition y Master Projection convergence. |
| `08_ROADMAP.md`, `09_OPEN_QUESTIONS.md` | Source convergence NEXT; runtime smoke después; tooling reorg diferido. |
| `10_MANAGER/` | No reabierto en este hito. |
| `11_ADA_GENERIC/` | ADA Generic 0.2.22 consume Master Projection genérico. |
| `12_SOURCE_STORAGE/` | Source Core permanece frozen; namespace/composition consumer convergence es NEXT. |
| `13_ADA_WEB/` | No reabierto. |
| `14_ADA_COMMAND_CENTER/` | Generic 0.1.1: durable local host + Master Projection implementados. |
| `15_WEB_PLATFORM/` | Gap transversal principal: Source/namespace ownership y dual-app runtime smoke posterior. |
| `16_KPI_BACKEND_RECOVERY/` | Conservado; no reabierto. |
| `17_DISTRIBUTION_AND_TOOLING/` | Root tooling genérico + tooling por scope; normalización futura diferida. |
| `18_UNIVERSITY/` | No revalidado. |

## Checkpoints del hito

```text
DUAL-APP-ENV-DETAIL-CONTRACT                     CLOSED / CURRENT
COMMAND-CENTER-DURABLE-RUNTIME-COMPOSITION       CLOSED / VERIFIED
DUAL-APP-MASTER-PROJECTION-CONVERGENCE            CLOSED / VERIFIED
ATLANTICUS-WEB-MASTER-PROJECTION-EXTRACTION       CLOSED / VERIFIED

SOURCE-NAMESPACE-AND-COMPOSITION-CONVERGENCE      PLANNED / NEXT
DUAL-APP-DURABLE-RUNTIME-SMOKE                    PLANNED / AFTER SOURCE
SCOPE-TOOLING-TOPOLOGY-NORMALIZATION              PLANNED / DEFERRED
```

Los PRECHECK de distribución anteriores pertenecen a artifacts generados antes de este hito.
No tratarlos como qualification de los paquetes actuales hasta regenerarlos.
