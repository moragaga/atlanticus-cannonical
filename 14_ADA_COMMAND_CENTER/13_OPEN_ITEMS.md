# ADA Command Center — Open Items

Estado: **CURRENT — C1/C2/C4, UX-01/UX-02 y naming físico de Alarm Configuration Projection CLOSED dentro de sus gates. Resource Preparation + startup gate es el siguiente foco único. Tool Catalog local, integración física, Live/Management/History, conflictos UX/contrato y distribución siguen OPEN/PLANNED.** Actualización: 2026-09-30.

## 1. Autoridad del corte

```text
atlanticus:main            fe606cbefb932211b8329df9285004f4933df41d
atlanticus-decisions:main  50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
canonical:main previo      c2f442b523fb4429f5a8a76c1e6919687016773f
```

Git continúa SOLO LECTURA durante este cierre documental. No se implementa trabajo nuevo aquí.

## 2. Estado consolidado por elemento

| Elemento | Estado | Evidencia o frontera |
|---|---|---|
| C1 catálogo/discovery/UI Tool con ownership Web | **CLOSED / CURRENT** | `web/tools/catalog`, `discovery-cosmos`, `catalog-manager`; `backend/tools` SUPERSEDED. |
| C2 `APPLICATION=ada-command-center`, Source Key Domain | **CLOSED / CURRENT** | Tres jobs con `job_key`/leases separados; Source Key `alarm-configuration`. |
| C2 `VOLUMEN_PATH` | **CLOSED contractual / UNVERIFIED físico** | Absoluta/manual; mismo montaje real multi-contenedor no demostrado. |
| Alarm Projection logical/resource contract | **CLOSED / CURRENT** | `logical_id=ada.command_center.alarms.configuration.projection`; PK `/partition_key`. |
| Alarm Projection physical name | **CLOSED / CURRENT** | `alarm-configuration`; nombre `ada-command-center-alarm-configuration-projection` SUPERSEDED. |
| Alarm Projection local root | **CLOSED / CURRENT** | `<base>/conciencia_situacional/command-center/projections/alarm-configuration/...`. |
| Alarm Projection Cosmos binding físico | **UNVERIFIED** | Container contract `alarm-configuration`; misma cuenta/base real Web/Materialization no acreditada. |
| C4 Delivery receptor CURRENT-only | **CLOSED / CURRENT** | Último CURRENT con pin/EFFECTIVE/READY exactos; Runtime FACTS v2 permanece. |
| UX-01 modal Save Draft | **CLOSED en implementación/tests** | Éxito cierra modal; error no. |
| UX-02 Rule/Message limit | **CLOSED en implementación/tests / conflicto documental previo permanece** | Git usa `1..11 | END_OF_SHIFT`; B.1 histórico conserva `1..12`. |
| Resource Preparation + startup gate | **PLANNED / NEXT** | Debe revisarse contra infraestructura ya existente; no implementado en CC. |
| Tool Catalog filesystem local | **NOT IMPLEMENTED / OPEN** | Host `local` continúa usando Storage para Tool Catalog. |
| Starter genérico con runtime/Home | **PLANNED posterior** | No iniciar antes de cerrar recursos/arranque. |
| C3 Qualification GREEN producer/evaluadores | **PLANNED / BLOCKED BY DESIGN** | JSON manual CURRENT; owner/evidencia reales ausentes. |
| C5 technical evidence/env restantes | **PLANNED / OPEN** | Owner/key/version reales sin decidir. |
| Docker artefactos/servicios independientes | **UNVERIFIED** | Suites locales no acreditan contenedores. |
| Alarm Source Blob ↔ Projection Cosmos E2E | **UNVERIFIED** | Adapters/contracts existen; no hay gate físico. |
| Live Delivery y `AlarmLiveProjection` | **PLANNED / NOT IMPLEMENTED** | C4 no genera Live. |
| Management Capture/Projection, History/Analytics | **PLANNED / separate** | No inferir de CURRENT/FACTS. |
| `alarms-materialization -> web/alarms/projection-cosmos` | **OPEN técnico heredado** | Dependency boundary no refactorizada. |
| Defectos UX de retención/validaciones/alertas | **OPEN** | Findings previos; fuera del siguiente incremento. |
| END_OF_SHIFT operacional | **UNVERIFIED / PLANNED** | Falta fuente real de shift end/effective_until UTC. |
| Python 3.14.7 vs metadata/tooling 3.14.2 | **OPEN** | Fuera de este incremento; afecta distribución/qualification limpia. |

## 3. Invariantes CURRENT preservados

- Source `AlarmConfigurationSnapshot` v3 con ToolDependencyManifest Cn congelado; `VALID_AT_SAVE != READY != EFFECTIVE`.
- `ALARM_CONFIGURATION_SOURCE_KEY = 'alarm-configuration'` en Domain.
- `APPLICATION=ada-command-center` común a los tres jobs, con `job_key`/lease separados.
- `VOLUMEN_PATH` manual/absoluta exige mismo medio físico real.
- Alarm Projection usa identidad física compartida `alarm-configuration`; local y Cosmos no mantienen nombres alternativos.
- El nombre físico anterior está SUPERSEDED y no requiere alias/migración legacy.
- Local Alarm Projection se deriva mediante `AdaStorageNamespace` y physical name; no mediante `Path.cwd()` implícito dentro del store.
- Cosmos Alarm Projection deriva nombre/PK del resource contract, no de env duplicada.
- READY íntegro reúne `runtime.json` + `delivery.json`; BLOCKED no reemplaza READY.
- Pin completo `source_key + result_id + manifest_sha256 + resolution_key`; WAL/EFFECTIVE sigue autoridad operacional.
- Runtime CURRENT v1 y FACTS v2 son productos distintos. Delivery consume únicamente último CURRENT y no construye Live.
- Tool Catalog en modo local **todavía no** es filesystem; no falsear ese estado.
- Live, Management e History/Analytics permanecen fronteras separadas.

## 4. Decisiones/refinamientos de este hito

### CURRENT / CLOSED

La identidad física de Alarm Configuration Projection fue refinada a:

```text
alarm-configuration
```

Rationale: el boundary de aplicación/Cosmos ya identifica Command Center; el resource physical name debe ser simple y comparable con la convención Storage. La identidad global sigue en `logical_id`.

La misma identidad física se usa en el adapter local bajo:

```text
conciencia_situacional/command-center/projections/alarm-configuration/
```

### SUPERSEDED

```text
ada-command-center-alarm-configuration-projection
```

No conservar compatibilidad legacy.

### PROPOSED / PLANNED, NO IMPLEMENTADO

La dirección para el siguiente incremento es reutilizar los mismos flujos de resolución/preparación de recursos para providers locales y durables y ejecutar un preparation worker/startup gate antes de habilitar consumidores. Debe comprobarse contra el código existente antes de introducir nuevas abstracciones.

## 5. Evidencia del incremento

```text
Commit integrado                               fe606cbefb932211b8329df9285004f4933df41d
Alarm Configuration Web                         123 PASS
Alarm Projection Cosmos                           5 PASS
Configuration Manager                            28 PASS
TOTAL                                            156 PASS
git diff --check                                 PASS
working tree después del commit                  limpio según git status --short aportado
```

Los errores masivos de collection observados durante una ejecución global de pytest no son regresiones de este incremento; las suites correctas fueron ejecutadas de forma aislada después.

## 6. Conflictos Implementation / Decisions / Canonical

### Resuelto por estos reemplazos canónicos

`04_CONFIGURATION_SCOPE.md` todavía declaraba:

```text
physical_name ada-command-center-alarm-configuration-projection
```

mientras `atlanticus@fe606cb...` ya implementa `alarm-configuration`. Al integrar este paquete, canonical queda alineado con implementación.

### Decisions

No se encontró en `atlanticus-decisions@50c2bb...` una ocurrencia indexada del nombre físico antiguo ni del nuevo. Por tanto, para este nombre concreto no se acredita una contradicción explícita con Decisions; la implementación actual es la autoridad material y este canonical documenta el delta. No inventar una decisión histórica inexistente.

### Conflictos heredados no resueltos aquí

- B.1 frozen `1..12` frente a Git `1..11 | END_OF_SHIFT`.
- Project baseline Python 3.14.7 frente a múltiples contratos/tooling actuales en 3.14.2.
- Materialization depende técnicamente de Web Projection Cosmos.
- Tool Catalog sigue Blob/Storage aun con manager provider `local`.
- C3/C5 y bindings físicos reales continúan sin cierre.

## 7. OPEN inmediato y razón

**Resource Preparation + startup gate — PLANNED / NEXT.** Es la siguiente frontera porque antes de generar/levantar un Starter necesitamos que recursos requeridos estén resueltos y asegurados de forma consistente y que los consumidores no arranquen antes de ese gate.

Debe comenzar auditando lo existente, no diseñando desde cero. En particular:

- cómo ADA Generic resuelve/prepara recursos hoy;
- qué resource contracts/nombres ya existen;
- cómo se aseguran roots locales, Blob containers, Cosmos database/containers;
- cómo se representa el readiness/startup dependency;
- cómo eliminar la excepción de Tool Catalog en `local` sin introducir un flujo paralelo.

No mezclar Home, Live, History, Management, C3, C5 ni nueva UX.

## 8. Documentación canónica de este cierre

Reemplazar exactamente estos archivos:

```text
14_ADA_COMMAND_CENTER/02_CURRENT_IMPLEMENTATION.md
14_ADA_COMMAND_CENTER/04_CONFIGURATION_SCOPE.md
14_ADA_COMMAND_CENTER/06_ENGINE_AND_PROJECTIONS.md
14_ADA_COMMAND_CENTER/11_GOLDEN_PATH.md
14_ADA_COMMAND_CENTER/13_OPEN_ITEMS.md
```

No se requiere actualizar otros documentos canónicos para registrar únicamente este hito. Si un refresh global futuro modifica Authority/Index, debe hacerse en otro foco y contra los HEAD vigentes.
