# Atlanticus Canonical Context — Index

Estado: **ÍNDICE POR ÁMBITO — checkpoint KPI histórico preservado + delta ADA operacional CURRENT 2026-09-29**

## Autoridad del checkpoint KPI anterior (HISTORICAL para ADA Datos operacionales)

Implementación publicada en aquel checkpoint:

```text
moragaga/atlanticus@d484569cbe0290f38f239481cde81b13a23deecf
```

Parent inmediato:

```text
dde1e3a114a04b22cc2118c347a7ed907852c06b
```

Tree:

```text
4c7c8209f2d0c670d3c6e8b5185b5af12172e591
```

Canonical inspeccionado antes de este reemplazo:

```text
moragaga/atlanticus-cannonical@a8c8c80ed3392cb189923d00bd5037e5965e2da5
```

`moragaga/atlanticus-decisions` permanece HISTORICAL.

Git permanece SOLO LECTURA para el asistente.

## Estado KPI / ADA Web relevante

```text
KPI-REGISTRY-CAPABILITY-CUTOVER                 CLOSED / VERIFIED / CURRENT
KPI-DEFINITION-CAPABILITY-CUTOVER               CLOSED / VERIFIED / CURRENT
KPI-RUNTIME-REPROCESS-CURRENT                   CLOSED / VERIFIED / CURRENT
KPI-DELIVERY-REGISTRY-CONSUMPTION               CLOSED / VERIFIED / CURRENT
KPI-TIMESERIES-REGISTRY-CONSUMPTION             CLOSED / VERIFIED / CURRENT
KPI-HISTORIAN-REPROCESS-CURRENT                 CLOSED / VERIFIED / CURRENT

ATLANTICUS-WEB-OBSERVABILITY-SERVICE            CLOSED / VERIFIED / CURRENT
ADA-WEB-KPI-COLLECTOR-CAPABILITY                CLOSED / VERIFIED / CURRENT
KPI-COLLECTOR-DEFINITION-ATTACHMENT             CLOSED / VERIFIED / CURRENT
KPI-COLLECTOR-REAL-WEB-SMOKE                    CLOSED / VERIFIED / CURRENT

ADA-GENERIC-COLLECTOR-OPERATIONAL-INTEGRATION   PLANNED / NEXT
```

## Índice relevante

| Archivo | Contenido | Estado |
|---|---|---|
| `01_CURRENT_STATE.md` | Estado implementado, validado y pendiente. | CURRENT |
| `02_ARCHITECTURE.md` | Fronteras arquitectónicas CURRENT. | CURRENT |
| `03_DECISIONS_CURRENT.md` | Decisiones activas y reglas de cutover. | CURRENT |
| `07_VALIDATION_BASELINE.md` | Evidencia observada de qualification. | CURRENT |
| `08_ROADMAP.md` | Orden de ejecución. | CURRENT |
| `09_OPEN_QUESTIONS.md` | Open items vigentes. | CURRENT |
| `11_ADA_GENERIC/` | ADA Generic, Collector cerrado y wiring operacional siguiente. | CURRENT / NEXT |
| `13_ADA_WEB/` | Web platform ADA, observability y collector qualification. | CURRENT |
| `16_KPI_BACKEND_RECOVERY/` | KPI backend recovery + Registry consumption. | CLOSED / CURRENT |

## Siguiente foco único del checkpoint KPI

```text
ADA-GENERIC-COLLECTOR-OPERATIONAL-INTEGRATION
PLANNED / NEXT
```

No volver a diseñar Collector. El próximo chat debe localizar la composición operacional real,
resolver Tool/Cosmos con los contratos CURRENT y montar el collector mediante el attachment ya
implementado.

---

## Actualización focal ADA Datos operacionales — 2026-09-29

**Alcance exclusivo:** esta adición no recalifica ni sustituye los checkpoints KPI y otros dominios anteriores. Los SHAs anteriores describen el contexto de su propio hito; para **Datos operacionales ADA** la implementación fue verificada separadamente en `moragaga/atlanticus:main@caced5d7711cf059d36ec61aecc9b3e9629bd41f`, contra canonical de partida `moragaga/atlanticus-cannonical:main@ec16bd2ccf0ae06065b8ee1d3a231ef4d2cbac57`.

- `10_MANAGER/10_ADA_OPERATIONAL_IDENTIFICATION_BOUNDARY.md`: **CURRENT**, backend/catalogo/asignaciones/Manager y distinción respecto a nueva UX.
- `10_MANAGER/11_ADA_OPERATIONAL_DATA_ROADMAP.md`: **CURRENT PLAN**, contratos OPEN y orden de incrementos del frente.
- `15_WEB_PLATFORM/14_ADA_OPERATIONAL_SESSION_AND_WARMUP.md`: **DECIDED DESIGN / PLANNED**, sesión Cosmos individual y warmup exclusivo de Profiles y catálogo operacional, nunca usuarios o asignaciones.

**Evidencia local comunicada:** 25 tests del scope operacional PASS y 13 tests del Manager en gate separado; `ruff check` detectó un `I001` pendiente. No es calificación CI/Azure/Entra. **Foco único de este frente:** completar el contrato técnico del snapshot consolidado, cuyo significado funcional ya quedó decidido (un archivo sobrescrito, sin versionado, solo usuarios con datos asignados), antes de implementarlo. Esta prioridad local no reemplaza la prioridad del frente KPI Collector indicada arriba.
