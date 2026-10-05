# Atlanticus Canonical Context — Index

Estado: **CURRENT — OPERATIONAL DATA FINAL CONTRACT + KPI MIGRATION CLOSED; DISTRIBUTION/TOOLING NEXT**

## Autoridad de este cierre

```text
Implementation        moragaga/atlanticus@777f3a0894a58f7275473ab34ce6b33cf767f9e7
Canonical pre-replace moragaga/atlanticus-cannonical@44d3c803f60d1a1630d3a3374a663447cfe21248
Decisions             moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Git                   SOLO LECTURA
```

## Estado por frente

| Ubicación | Estado relevante |
|---|---|
| `01_CURRENT_STATE.md` | Operational Data final contract CLOSED / VERIFIED; KPI migrated; Alarm Runtime BLOCKED intentionally. |
| `02_ARCHITECTURE.md` | Single Operational Data consumer path frozen: `DataInputSpec -> DataInputContext`. |
| `03_DECISIONS_CURRENT.md` | Legacy Operational Data consumer contract removed; no adapters. |
| `04_ALARM_ENGINE/` | Alarm domain remains valid; executable Alarm Runtime is BLOCKED pending migration to the new data-input contract. |
| `16_KPI_BACKEND_RECOVERY/` | KPI Runtime now consumes the final Operational Data input contract; focused qualification green. |
| `17_DISTRIBUTION_AND_TOOLING/` | NEXT single focus: artifact generation, qualification, `.env.detail` audit and distribution. |

## Checkpoints

```text
OPERATIONAL-DATA-INPUT-CONTRACT                 CLOSED / VERIFIED / CURRENT
OPERATIONAL-DATA-LEGACY-CONTRACT-REMOVAL        CLOSED / VERIFIED / REMOVED
KPI-RUNTIME-DATA-INPUT-MIGRATION                CLOSED / VERIFIED / CURRENT
ALARM-RUNTIME-DATA-INPUT-MIGRATION              BLOCKED / PLANNED
DISTRIBUTION-AND-TOOLING-NORMALIZATION          PLANNED / NEXT
```

## Siguiente frontera única

```text
ATLANTICUS-DISTRIBUTION-AND-TOOLING-FINAL-QUALIFICATION
```

Objetivo del próximo chat:

- retomar generación de artifacts;
- comprobar que todos se generan correctamente;
- auditar `.env.detail` campo por campo;
- identificar valores requeridos, opcionales, secretos y system-derived/system-assigned;
- regenerar y calificar la versión distribuible;
- no mezclar Alarm Runtime ni otros frentes.
