# Atlanticus Canonical Context — Index

Estado: **CURRENT EXECUTION CHECKPOINT**

## Autoridad

Implementación publicada CURRENT:

```text
moragaga/atlanticus@d71e94d12fa31a986b3ecc0262fbbb6ef2e4a3dd
```

Parent inmediato:

```text
107c7570061e0d31828b1d3e9b9fc6336a698809
```

Tree:

```text
41c299861d14a9691cbd3461dbca8bb466dfc156
```

Canonical inspeccionado antes de este reemplazo:

```text
moragaga/atlanticus-cannonical@a3e77aafe5e97bd4e2e10a9d9b24be9ff0486471
```

`moragaga/atlanticus-decisions` permanece HISTORICAL.

Git permanece SOLO LECTURA para el asistente.

## Índice

| Archivo | Contenido | Estado |
|---|---|---|
| `00_AUTHORITY.md` | Autoridad de fuentes y conflictos. | CURRENT |
| `01_CURRENT_STATE.md` | Estado implementado, validado y pendiente. | CURRENT |
| `02_ARCHITECTURE.md` | Fronteras y dependencias. | CURRENT |
| `03_DECISIONS_CURRENT.md` | Decisiones activas y reglas de cutover. | CURRENT |
| `04_ALARM_ENGINE/` | Alarm Engine + qualification/preservation. | CURRENT + PRESERVATION |
| `05_ENGINEERING_BASELINE.md` | Baseline técnica. | CURRENT |
| `06_OPERATING_MODEL.md` | Modelo operativo/deployment. | CURRENT |
| `07_VALIDATION_BASELINE.md` | Evidencia de qualification y límites. | CURRENT |
| `08_ROADMAP.md` | Orden de ejecución. | CURRENT |
| `09_OPEN_QUESTIONS.md` | Open items vigentes. | CURRENT |
| `10_MANAGER/` | Manager y compositions administrativas. | CURRENT |
| `11_ADA_GENERIC/` | ADA Generic y cadena Tool → KPI → runtime. | CURRENT / COLLECTOR BLOCKED |
| `12_SOURCE_STORAGE/` | Source/Projection exact-release y storage. | CURRENT |
| `13_ADA_WEB/` | ADA Web, KPI Registry y KPI Definition. | CURRENT |
| `14_ADA_COMMAND_CENTER/` | Command Center y Alarm ownership. | CURRENT DIRECTION |
| `15_WEB_PLATFORM/` | Web platform, Users, Profiles, Access, Navigation y runtime. | CURRENT |
| `16_KPI_BACKEND_RECOVERY/` | KPI backend recovery + Registry consumption. | PLANNED / NEXT |
| `17_DISTRIBUTION_AND_TOOLING/` | Generators, artifacts, scripts, docs y services. | CURRENT DIRECTION |
| `18_UNIVERSITY/` | Casos pedagógicos reales. | CURRENT DIRECTION |
| `ATLANTICUS_ENGINEERING_RULES.md` | Reglas de ingeniería. | CURRENT |
| `BASELINE_CLOSURE.md` | Qué queda congelado y qué no. | CURRENT |

## Hitos KPI cerrados

```text
KPI-REGISTRY-CAPABILITY-CUTOVER
CLOSED / VERIFIED / CURRENT

KPI-DEFINITION-CAPABILITY-CUTOVER
CLOSED / VERIFIED / CURRENT

KPI-MANAGER-REGISTRY-WIRING
CLOSED / VERIFIED / CURRENT

KPI-MANAGER-DEFINITION-WIRING
CLOSED / VERIFIED / CURRENT
```

## Próxima secuencia

```text
1. KPI-RUNTIME-REPROCESS-CURRENT
2. KPI-DELIVERY-REGISTRY-CONSUMPTION
   + KPI-TIMESERIES-REGISTRY-CONSUMPTION
3. KPI-HISTORIAN-REPROCESS-CURRENT
4. ADA-GENERIC-COLLECTOR-CLOSURE
```

`ADA-GENERIC-COLLECTOR-CLOSURE` permanece bloqueado hasta cerrar la cadena backend KPI.
