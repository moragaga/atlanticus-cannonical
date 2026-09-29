# ADA Command Center — Open Items

Estado: **CURRENT — C1/C2/C4 y UX-01/UX-02 CLOSED dentro de sus gates; Starter genérico propio con runtime y Home mínima PLANNED como siguiente foco; defectos UX, reconciliación contractual, Live/Management/History e integración física siguen OPEN o PLANNED**. Actualización: 2026-09-29.

## 1. Autoridad del corte

```text
atlanticus:main           2e7500a6b8b4d5bbdad26d807abfa57936db99d5
atlanticus-decisions:main 50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
canonical:main previo    2e8bbf4780cafc4cea3b18351861aa97a4fb0053
```

**Precaución documental:** el canónico remoto es anterior a la integración del reemplazo C4 y al cierre UX-01/UX-02. Los reemplazos C4 generados en un chat anterior no constan integrados al repositorio; contrastar cualquier archivo local posterior antes de utilizar este paquete. El presente cierre es documental: Git SOLO LECTURA, sin implementación nueva ni validación Docker/Azure adicional.

## 2. Estado consolidado por elemento

| Elemento | Estado | Evidencia o frontera |
|---|---|---|
| C1 catálogo/discovery/UI Tool con ownership Web | **CLOSED / CURRENT** | `web/tools/catalog`, `discovery-cosmos`, `catalog-manager`; `backend/tools` SUPERSEDED; B1d local histórico. |
| C2 `APPLICATION=ada-command-center`, Source Key Domain | **CLOSED / CURRENT** | Tres jobs con distintos `job_key`/leases; Source Key literal `alarm-configuration` en Domain. |
| C2 `VOLUMEN_PATH` | **CLOSED contractual / UNVERIFIED físico** | Configuración absoluta manual; mismo montaje físico multi-contenedor no demostrado. |
| C2 recurso Cosmos de Projection | **CLOSED en código / UNVERIFIED físico** | Web/Materialization usan resource contract; conexión real a misma cuenta/base no acreditada. |
| C4 Delivery receptor CURRENT-only | **CLOSED / CURRENT** | Git y cierre anterior: último CURRENT, valida pin/EFFECTIVE/READY; Runtime FACTS v2 permanece. Los reemplazos canónicos C4 anteriores requieren reconciliación documental. |
| UX-01 modal Save Draft | **CLOSED en implementación/tests** | Éxito cierra modal; errores no deben cerrarlo; 122 PASS previos reportados en Web. |
| UX-02 límite Rule/Message | **CLOSED en implementación/tests** | `1..11 | END_OF_SHIFT`, Source schema v3 inalterado; commit `2e7500a...`, 20 archivos, suites 59+64+123=246 PASS. |
| Arranque Web de prueba sin emuladores | **CLOSED para prueba local aislada** | Lanzador EXTERNO, host real, stores archivo, una Tool Process sintética; NO Starter/host distribuido. |
| Crear familias, Rules, Messages; asignar y guardar | **VERIFIED / CLOSED básico** | Aceptación manual reportada por el usuario; no implica recuperación tras reinicio ni Product UX complete. |
| Defectos de retención de valores, validaciones, alertas | **OPEN** | Findings observados; sin causa ni regression gate visual específicos; pueden afectar calidad de borrador. |
| UX END_OF_SHIFT selección manual/operación real | **UNVERIFIED / PLANNED** | Tests estáticos y UI implementados; falta fuente de hora real, UTC y gestión E2E. |
| B.1 frozen deactivation `1..12` vs Git `1..11 | END_OF_SHIFT` | **CONFLICT / OPEN** | Mismo campo `max_duration_hours` y Source v3; exige decisión documental/versionado formal. |
| Python baseline 3.14.7 frente a metadata `==3.14.2` | **OPEN** | El prompt de shell indicaba 3.14.7, pero no se verificó el intérprete seleccionado por `uv run`; distribución limpia no cualificada. |
| Host temporal nativo sin dependencia Blob en `local` | **NOT IMPLEMENTED / separate** | `__main__` real usa Tool Catalog Blob en ambos providers; no adoptar fixture como producción. |
| Starter genérico con runtime/home | **PLANNED / NEXT** | Meta del usuario para próximo debate, sin implementación aquí. |
| C3 Qualification GREEN producer/evaluadores | **PLANNED / BLOCKED BY DESIGN** | JSON manual CURRENT, productor/verificadores reales sin identificar. |
| C5 contrato technical evidence/env restantes | **PLANNED / OPEN** | Owner/key/version reales sin decidir; no inventar variables. |
| Docker artefactos/servicios independientes | **UNVERIFIED / separate** | Regresión local no implica equivalencia física en contenedores. |
| Alarm Source Blob ↔ Projection Cosmos E2E | **UNVERIFIED / separate** | Adapters/contracts existen, no hay gate físico de binding. |
| Live Delivery y `AlarmLiveProjection` | **PLANNED / NOT IMPLEMENTED** | Receptor C4 no enriquece ni publica Live. |
| Management Capture/Projection, History/Analytics | **PLANNED / separate** | No inferir de CURRENT ni FACTS. |
| `backend/processes/alarms-materialization → web/alarms/projection-cosmos` | **OPEN técnico heredado** | Frontera de ownership sin refactor autorizado. |
| Independencia visual targets ↔ routing | **CONFLICT / OPEN** | Contrato visual y sincronización actual de editor divergen; resolver en UX propio, no Starter. |

## 3. Invariantes CURRENT preservados

- Source `AlarmConfigurationSnapshot` v3 con ToolDependencyManifest Cn congelado. `VALID_AT_SAVE != READY != EFFECTIVE`, sin latest Tool reinterpretando Rn/Cn.
- `ALARM_CONFIGURATION_SOURCE_KEY = 'alarm-configuration'` en Domain; host Web transforma a `SourceKey` técnico. C2 comparte APPLICATION entre jobs sin compartir `job_key`/lease; `VOLUMEN_PATH` manual/absoluta exige mismo medio físico, no solo la misma string.
- Cosmos Alarm Projection físico derivado de resource contract, no `.env` duplicado; contenedor Blob permanece ambiental y conexiones Tool Cosmos pueden ser múltiples/nombradas.
- READY íntegro reúne `runtime.json` y `delivery.json` + manifest; BLOCKED no reemplaza READY. Pin completo `source_key + result_id + manifest_sha256 + resolution_key`; WAL/EFFECTIVE es autoridad operacional.
- Runtime CURRENT v1 completo/reemplazable y FACTS v2 inmutables encadenados son productos distintos. Delivery C4 consume **únicamente último CURRENT**; no elimina FACTS Runtime ni construye Live.
- `enabled=false` implica `max_duration_hours=None, approval_required=false`. Override Message ausente hereda; presente reemplaza regla completa. Configuración actual `1..11 | END_OF_SHIFT`; no convertir silenciosamente `12` a fin del turno.
- Familias derivadas, sin Family durable vacía; routing `PROCESS → INTEGRATED_OPERATIONS → STRATEGIC → END`. Visual targets/routing son fronteras conceptuales distintas aunque editor actual pueda sincronizarlos.
- Live Projection, Management Projection e History/Analytics siguen separadas; Web no calcula priority/routing, interpolación de cause ni usa WAL como API.

## 4. OPEN UX observados y alcance de prueba

**Valores perdidos:** el usuario vio campos que se borran durante edición. No se identificó callback ni patrón de reproducción. **No** congelar el comportamiento como aceptable: su prioridad se decidirá en incremento UX si bloquea escenarios reales o antes de producción.

**Validaciones intermitentes:** algunos mensajes de validación desaparecen. Falta determinar condiciones/state; no suponer que los validators del Domain fallaron, ya que las suites pasaron.

**Alertas persistentes:** avisos `success`/`warning`/`danger` permanecen visibles. No inventar temporizador ni patrón genérico sin decisión UX y diagnóstico.

**Aceptación parcial:** guardar familias/Rules/Messages y asignarlas quedó **CLOSED** en browser aislado, pero no se acreditaron reinicio/recovery, edición de todas las secciones, error matrix, selección manual de END_OF_SHIFT con round-trip, ni responsive visual definitivo. Estos OPEN no revocan los gates de componentes y tampoco constituyen aprobación productiva de la UI.

## 5. OPEN de contrato y operación

**Fin del turno:** el editor implementa el valor estático `END_OF_SHIFT`. El Web operacional debe obtener el término real del proceso/turno, con semántica y timezone acordados, y enviar `effective_until` UTC al Core; no hay Management Capture/Web operacional implementado que cierre este camino. Decisions B.1 DESIGN FROZEN todavía declara máximo numérico `1..12` y cálculo conceptual limitado por `shift_end`; Git actual usa `1..11 | END_OF_SHIFT` en mismo campo. La discrepancia necesita decisión formal antes de consumidores adicionales.

**Source v3 compatibilidad:** codec de configuración acepta `str` tagged en `max_duration_hours` sin version bump; impacto en snapshots históricos `12`/consumidores externos es **UNVERIFIED**. El usuario instruyó no dejar legacy: resolver como contrato, no meter fallback/alias no autorizado ni migración ficticia.

**C3 qualification:** automatizar requiere fuente GREEN y evaluadores reales autorizados; la qualification mediante intervención humana/manual es un contrato legítimo mientras tanto. **C5 evidence/env:** registrar owner/key/version verificables antes de llenar plantillas.

**Integración física/distribución:** verificar package/runtime, `requires-python`, nombres/paths/env existentes, cuenta/base Cosmos común y montaje físico compartido entre jobs. Ninguno se acredita por suites UX ni por host sin emuladores.

## 6. Conflictos canónico / Decisions / implementación

1. Canonical remoto `15_...` y `18_...` describen desactivación `1..12`, modal y UX de fin de turno todavía OPEN. Git y gates de este hito los hacen CURRENT/CLOSED **sólo en editor estático**; cálculo operacional sigue OPEN.
2. Canonical remoto `00/02/03/04/06/11/12/13/16/17/18` refleja C4 PLANNED/CURRENT+FACTS cuando Git ya tiene C4 CURRENT-only. Los reemplazos C4 de otro cierre existen como trabajo preparado, **no** se acreditó su integración. No sobrescribir su detalle al combinar estos reemplazos UX.
3. B.1/B.2 Decisions históricos sobre SharePoint/topología, Special Cascade y Messages inactive conservan conflictos heredados; no reescribir o resolver implícitamente con Starter.
4. Project fija Python 3.14.7 mientras varios `pyproject.toml` de Command Center contienen `requires-python==3.14.2`; compatibilidad de instalación/distribución aislada sigue OPEN.
5. El host `__main__` en `local` usa Tool Catalog Blob, pero el lanzador aislado externo usa `TestOnlyFileCatalog`: ninguno de los dos hechos significa que el Starter genérico actual ya exista.

## 7. Documentos y siguiente frontera

Este paquete contiene reemplazos completos de `00_INDEX.md`, `03_WEB_APPLICATION.md`, `11_GOLDEN_PATH.md`, `13_OPEN_ITEMS.md`, `15_ALARM_CONFIGURATION_AUTHORING_MODEL.md`, `18_ALARM_AUTHORING_UX_AND_VISUAL_PRESENTATION.md`. Integrar sólo tras comprobar diff contra canonical local y otros reemplazos C4 existentes. Para sincronización canónica global **todavía** conciliar `02_CURRENT_IMPLEMENTATION.md`, `04_CONFIGURATION_SCOPE.md`, `06_ENGINE_AND_PROJECTIONS.md`, `12_SOURCE_LEDGER.md`, `16_ALARM_LIVE_DELIVERY_CONTRACT.md`, `17_DOMAIN_OWNERSHIP_AND_MIGRATION.md` con el paquete C4 previo, sin reconstruirlo por inferencia. `00_AUTHORITY.md` y `01_CURRENT_STATE.md` raíz pertenecen a un refresh global del Project, no a este incremento UX.

**PLANNED / único próximo foco:** debate/diseño del Starter genérico **propio** de Command Center con composición de runtime existente y Home mínima que permita acreditar un recorrido real para ejecutar alarmas. No Navigation, Users, Profiles, dashboard complejo, History ni nueva UX en el mismo incremento. Antes de construir frontend, definir cuál es el primer gate del Engine CURRENT/Delivery receptor actual y el eventual contrato Live que sería necesario para mostrar alarmas operacionales, sin fingir que existe.
