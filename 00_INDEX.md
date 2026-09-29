# Atlanticus Canonical Context — Index

Estado: **ÍNDICE POR ÁMBITO — checkpoints históricos preservados + delta focal Manager M01 CLOSED / ADA M02 PLANNED (2026-09-29)**. Las fechas y SHAs de cada apartado identifican **su propio corte**, no un HEAD global intercambiable.

## Autoridades de este delta focal

```text
Implementación CURRENT inspeccionada: moragaga/atlanticus:main@9cc2cebe595ef1341830374ad2bb3c61baf6f5a2
M01 parent:                         2e7500a6b8b4d5bbdad26d807abfa57936db99d5
Decisiones:                         moragaga/atlanticus-decisions:main@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical leído antes de entrega:   moragaga/atlanticus-cannonical:main@2e8bbf4780cafc4cea3b18351861aa97a4fb0053
```

**Verificación adicional al finalizar:** canonical avanzó a `77874b70f2d1fa2a3b8df988a2b59d7e9200dee8`; la comparación con el corte leído muestra 12 archivos cambiados exclusivamente bajo `14_ADA_COMMAND_CENTER/`, sin solapamiento con los siete reemplazos de este paquete. No se recalifica ese otro frente.

Git es SOLO LECTURA para el asistente. Los documentos de este paquete son **reemplazos candidatos**: su creación local no significa incorporación al repositorio canonical. Revalidar HEAD antes de integrarlos.

## Índice por frente, sin recalificar otros ámbitos

| Ubicación | Contratos / estado |
|---|---|
| `01_CURRENT_STATE.md` | Checkpoints de Storage/Tool/ADA Generic existentes; SHAs históricos pertenecen a su propio corte. |
| `02_ARCHITECTURE.md` | Ownership, fronteras y composiciones genéricas; no alteradas por M01. |
| `03_DECISIONS_CURRENT.md` | Reglas globales y decisiones por frente; leer junto a `atlanticus-decisions`, nunca usar memoria como autoridad. |
| `04_ALARM_ENGINE/` | Alarm Engine y sus gates propios; M01 no los califica. |
| `05_ENGINEERING_BASELINE.md`, `06_OPERATING_MODEL.md` | Python/uv, Git read-only, diseño incremental, mirrors y pruebas. |
| `07_VALIDATION_BASELINE.md` | Evidencia de otros cortes; para los gates específicos M01 consultar `10_MANAGER/03_WORKFLOW_AND_SESSION.md`. |
| `08_ROADMAP.md`, `09_OPEN_QUESTIONS.md` | Roadmaps y abiertos por frente. No interpretar un NEXT de otro dominio como orden universal. |
| `10_MANAGER/` | **ACTUALIZADO EN ESTE PAQUETE**: M01 genérico cerrado; M02 ADA operacional siguiente diseño; demás áreas no reabiertas. |
| `11_ADA_GENERIC/`, `12_SOURCE_STORAGE/`, `13_ADA_WEB/` | Sus propios checkpoints conservados. |
| `14_ADA_COMMAND_CENTER/`, `15_WEB_PLATFORM/` | Frentes paralelos; sesión/warmup operacional aún pendiente. |
| `16_KPI_BACKEND_RECOVERY/`, `17_DISTRIBUTION_AND_TOOLING/`, `18_UNIVERSITY/` | Frentes autónomos; no modificados por M01. |

## Checkpoint KPI histórico — conservar su propio NEXT

```text
KPI-REGISTRY-CAPABILITY-CUTOVER                 CLOSED / checkpoint anterior
KPI-DEFINITION-CAPABILITY-CUTOVER               CLOSED / checkpoint anterior
KPI-RUNTIME-REPROCESS-CURRENT                   CLOSED / checkpoint anterior
KPI-DELIVERY-REGISTRY-CONSUMPTION               CLOSED / checkpoint anterior
KPI-TIMESERIES-REGISTRY-CONSUMPTION             CLOSED / checkpoint anterior
KPI-HISTORIAN-REPROCESS-CURRENT                 CLOSED / checkpoint anterior
ATLANTICUS-WEB-OBSERVABILITY-SERVICE            CLOSED / checkpoint anterior
ADA-WEB-KPI-COLLECTOR-CAPABILITY                CLOSED / checkpoint anterior
KPI-COLLECTOR-DEFINITION-ATTACHMENT             CLOSED / checkpoint anterior
KPI-COLLECTOR-REAL-WEB-SMOKE                    CLOSED / checkpoint anterior
ADA-GENERIC-COLLECTOR-OPERATIONAL-INTEGRATION   PLANNED / NEXT DEL FRENTE KPI
```

En su checkpoint anterior, la implementación publicada se identificó como `moragaga/atlanticus@d484569cbe0290f38f239481cde81b13a23deecf`, parent `dde1e3a114a04b22cc2118c347a7ed907852c06b`, tree `4c7c8209f2d0c670d3c6e8b5185b5af12172e591`. Es **HISTORICAL para M01/M02**; no vuelve a calificarse aquí.

## Delta anterior: ADA Datos operacionales — preservado y refinado

El corte focal anterior se basó en `atlanticus:main@caced5d7711cf059d36ec61aecc9b3e9629bd41f`; confirmó dominio operacional implementado, Sources separados, Cosmos projections y una interfaz Manager anterior. Las 25 pruebas operacionales y 13 Manager comunicadas pertenecen a aquel gate. Permanecen pendientes snapshot consolidado, sesión Entra y warmup; ninguno se ha implementado como consecuencia de M01. El esquema y recovery técnico del snapshot siguen OPEN.

La UI antigua y sus nombres/orden de pestañas permanecen en el código ADA actual, hasta M02. Una revisión documental anterior acordaba «Datos operacionales» / «Asignación», mientras que el diseño posterior para M02 propone «Asignaciones» / «Catálogo de cargos». Es **CONFLICT DOCUMENTAL** a reconciliar, no cambio ya implementado.

## Nuevo delta: M01 Manager Companion View — CLOSED / CURRENT

`atlanticus:main@9cc2cebe` publica la extensión genérica `ManagerCompanionView`, opcional en `ManagerModule`; solo el módulo principal conserva su workflow administrativo. El usuario ejecutó **82 pruebas Manager PASS** y `git diff --check` PASS en entorno local; tras corregir dos imports nuevos en producción y espejo quedaron **seis** incidencias Ruff preexistentes, no Ruff PASS general. Se comprobó posteriormente que `origin/main` ya contiene el commit y exactamente los 12 archivos del hito. No se ejecutó CI remoto ni prueba visual ADA M02.

**Foco exclusivo del chat siguiente para este frente: M02 — diseño e integración de Datos operacionales ADA con M01**, sin modificaciones de backend no autorizadas y después de formalizar el orden de vistas. Referencias operativas:

- `10_MANAGER/00_INDEX.md`
- `10_MANAGER/03_WORKFLOW_AND_SESSION.md`
- `10_MANAGER/10_ADA_OPERATIONAL_IDENTIFICATION_BOUNDARY.md`
- `10_MANAGER/11_ADA_OPERATIONAL_DATA_ROADMAP.md`

No declarar M02 CLOSED antes de reconfigurar ADA, probar publicaciones/reintentos y completar la validación visual.
