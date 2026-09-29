# Manager — Canonical Index

Estado: **CURRENT / checkpoint focal M01 CLOSED y M02 PLANNED — 2026-09-29**. Los checkpoints históricos de otros módulos siguen siendo históricos para su propio alcance; no derivar de ellos el HEAD actual.

## Autoridad focal de este índice

```text
Implementación actual inspeccionada:   moragaga/atlanticus:main@9cc2cebe595ef1341830374ad2bb3c61baf6f5a2
Parent de M01:                        2e7500a6b8b4d5bbdad26d807abfa57936db99d5
Decisions leídas:                     moragaga/atlanticus-decisions:main@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical previo leído para cierre:   moragaga/atlanticus-cannonical:main@2e8bbf4780cafc4cea3b18351861aa97a4fb0053
```

M01 contiene 12 archivos exclusivos de `web/capabilities/manager`, publicados en `atlanticus:main`. Su aceptación automatizada local es **82 pruebas Manager PASS**, espejos AST incluidos; no CI ni E2E visual. El código ADA Datos operacionales permanece con su `ManagerEntry` y publicación inmediata de cargos: M02 **todavía no** está implementado.

## Índice de la documentación Manager

| Archivo | Contenido y estado |
|---|---|
| `01_APPLICATION_BOUNDARY.md` | Ownership y separación Manager genérico vs consumidores ADA — CURRENT. |
| `02_NAVIGATION_AND_HOME.md` | Home, sidebar, header y navegación — CURRENT; sus checkpoints visuales tienen evidencia histórica propia. |
| `03_WORKFLOW_AND_SESSION.md` | Contrato ManagerModule/Entry, Source/Projection y **M01 Companion View** — CURRENT / M01 CLOSED. |
| `04_TOOL_CONFIGURATION.md` | Tool configuration y sus Sources/Projections — frontera independiente. |
| `05_SOURCE_BLOB_HANDOFF.md` | Lectura/publicación Source y proyección desde Manager — frontera independiente. |
| `06_TESTING_BOUNDARY.md` | Pruebas de comportamiento y validación visual — CURRENT POLICY. |
| `07_SOURCE_LEDGER.md` | Checkpoints anteriores Manager/ADA y evidencia histórica; no reescribir resultados retrospectivamente. |
| `08_BOOTSTRAP_AND_ACCESS.md` | Acceso, Users y Master — fronteras independientes. |
| `09_ADA_COMPONENT_LINKS.md` | Configuración de links del componente ADA — frontera independiente. |
| `10_ADA_OPERATIONAL_IDENTIFICATION_BOUNDARY.md` | Dominio/persistencia y UI anterior de Datos operacionales, frente a adopción M01/M02 — CURRENT / GAP identificado. |
| `11_ADA_OPERATIONAL_DATA_ROADMAP.md` | Único foco del siguiente chat M02 y backlog separado de snapshot, sesión, warmup — CURRENT PLAN. |
| `../15_WEB_PLATFORM/14_ADA_OPERATIONAL_SESSION_AND_WARMUP.md` | Contrato anterior de sesión/warmup, **no** implementado por M01. |

## Checkpoints históricos preservados

Manager y Users conservan ownership genérico. El sistema registra `ManagerModule` con Source/Projection y `ManagerEntry` para entradas administrativas autónomas. `/manager` es Home real y todas las superficies derivan de un registry común sujeto a autorización; Home/cabecera/sidebar no dependen de ADA. La prueba manual histórica de Profiles, Accesos, header local, Usuarios y Master conserva su ámbito/fecha; no convertirla en pruebas del nuevo M01 sobre ADA. Los contratos históricos de Users Backup/Restore y Master Projection externo no cambian por M01. Un hallazgo histórico sobre autorización de Navigation exige revalidación en su propio alcance, no alias nuevos.

La afirmación histórica «Identificación operacional ADA PLANNED / sin implementación» está **SUPERSEDED solo para ese dominio**: ya hay dominio, Source de catálogo y por usuario, proyecciones Cosmos, servicio y UI propia. Sin embargo, el snapshot consolidado, warmup, wiring de sesión y nueva UI M02 **no** están implementados.

## M01 — CLOSED / CURRENT

`ManagerCompanionView` aporta una vista opcional solo de presentación, externa al workflow de `ManagerModule`; selección inicial `'module' | 'companion'`. Otros módulos sin companion conservan su layout anterior. La prueba local publicada junto al commit contiene cuatro tests específicos; suite completa Manager reportada **82 PASS**; seis hallazgos Ruff del baseline permanecen OPEN / SEPARATE. No hay evidencia visual final en ADA.

## Frontera de traspaso: M02 — PLANNED / DESIGN

El chat siguiente tendrá **un solo foco**: sustitución limpia del `ManagerEntry` operacional de ADA por la composición que reutilice `ManagerModule + ManagerCompanionView` y sus workflows de catálogo ya existentes, conservando la publicación/proyección individual inmediata de asignaciones. Primero reconciliar el conflicto de labels/orden entre el canonical histórico («Datos operacionales» / «Asignación») y la propuesta posterior («Asignaciones» / «Catálogo de cargos», companion inicial). Ningún archivo ADA de M02 se escribió durante M01.

Este NEXT local al frente Manager no altera el NEXT propio del checkpoint KPI Collector ni la secuencia de snapshot, sesión, warmup o Alarm Engine. Ver `11_ADA_OPERATIONAL_DATA_ROADMAP.md` para riesgos y pruebas.
