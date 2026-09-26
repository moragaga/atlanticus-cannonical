# Atlanticus — Authority

Estado: **CURRENT / AUDITED CHECKPOINT 2026-09-26**

## Autoridades activas

| Fuente | Referencia comprobada | Papel |
|---|---|---|
| `moragaga/atlanticus:main` | `411aea44ac60c09d2b07ce41d34c3f378788b97b` | Realidad implementada a este corte. |
| `moragaga/atlanticus-cannonical:main` | `83cd871c8418e37d2c29dff30e2ea5ef54bda4a0` | Baseline documental inspeccionado **antes** del reemplazo local propuesto. |
| `moragaga/atlanticus-decisions:main` | `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e` | Decisiones y genealogía histórica. |

Estos SHA describen el corte auditado; no afirman que los repositorios permanezcan inmutables después de este documento. Git es **SOLO LECTURA** sin autorización explícita de escritura.

Jerarquía de trabajo:
1. `atlanticus:main`: código y contratos efectivamente implementados.
2. `atlanticus-cannonical:main`: estado y contratos documentados vigentes; contrastar con el código.
3. Qualification y tests: evidencia verificable, asociada a su revisión.
4. Decisiones explícitas del Project todavía sin formalizar: delta temporal.
5. `atlanticus-decisions`: intención y evidencia histórica; no elevar un borrador a realidad.
6. Historial conversacional: pista, nunca autoridad suficiente.

Ante contradicción, consignar **CONFLICT**; no reconciliar silenciosamente.

## Cierre de este hito: Alarm Configuration y routing

**CURRENT / IMPLEMENTED EN `atlanticus@411aea44`:**

- Dominio transversal de alarmas y `ToolDependencyManifest` en `domain/tools`.
- `AlarmConfiguration(rules, messages)` y `AlarmConfigurationSnapshot(configuration, tool_dependencies)`; Source schema `3`.
- Captura exacta del subconjunto Tool referenciado, incluso Rules inactivas y pasos deshabilitados; cada referencia conserva display name, source release, kind y `ToolStructure` de la revisión confirmada correspondiente.
- Alarm-specific workspace pin `_confirmed_tool_catalog_revision` y bloqueo ante drift entre validación y publicación. El Manager genérico no es dueño de esta correlación.
- Source/base projection de `AlarmConfigurationSnapshot`, stores local y Cosmos, codec y composición de providers local/blob y local/cosmos.
- Pure B.2 resolver sin I/O con `READY | BLOCKED`, hallazgos y producción **atómica** de Runtime y Delivery para la misma `AlarmResolutionKey`.
- Política compartida `next_routing_tool_kind` y comprobación de dirección en Materialization; la Web utiliza la misma política para las opciones del editor. Las Tools Strategic están disponibles para **routing**, no como visual targets.

**VERIFIED mediante ejecución aportada por el usuario antes del commit de cierre:** pruebas, Ruff y formatter de dominio Alarm, Materialization y Web Alarm Configuration tras sus respectivos incrementos. El diff entre `3413b5...` y `411aea...` contiene los cambios del último incremento; el commit de cierre fue comprobado por lectura en Git. No hay evidencia en este hito de una nueva ejecución de todas las suites *después* de `411aea...`, ni de una prueba host/browser integral posterior al routing.

**NO declarar CLOSED end-to-end:** las pruebas unitarias verdes no demuestran publicación real hacia Cosmos/Blob, carga operacional desde Cosmos, generación persistente de artefactos, descarga, Runtime Adoption ni consumo productivo.

## Tool authority CURRENT

```text
upstream Tool projections/Cosmos + prior Storage state
 -> reconciliation/certification
 -> Confirmed Tool Catalog Cn
 -> Storage
 -> END
```

No añadir como requisito una nueva proyección `Confirmed Tool Catalog -> Command Center Cosmos`.

## Alarm Configuration durable/source authority CURRENT

```text
AlarmConfiguration
  rules
  messages

AlarmConfigurationSnapshot
  configuration
  tool_dependencies: ToolDependencyManifest

source document_type = ada_command_center_alarm_configuration_release
source schema_version = 3
```

`confirmed_tool_catalog_revision` procede de `tool_dependencies.revision`; no duplicar ni recalcular. Source schema v2 está **SUPERSEDED**: no crear reader legacy.

```text
Save Draft -> pin Cn
Validate -> intrinsic + exact revision/tool existence
Publish -> recheck Cn + freeze referenced ToolDependencyManifest
Alarm source release Rn -> frozen Tool evidence Cn
```

Una nueva revisión Tool por sí sola no reinterpreta la release Alarm previa. `VALID_AT_SAVE != READY != EFFECTIVE` y `INVALID != REMOVED`.

## Routing CURRENT / FROZEN

```text
PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC
```

Sólo se permiten transiciones hacia el siguiente nivel inmediato. Prohibidos retrocesos, mismo nivel y saltos. Strategic es terminal. No se exige crear destinos: C1 y C2 pueden permanecer únicamente en origen; C3 siempre usa sólo origen. C1 escala inmediatamente; C2 usa esperas positivas entre pasos habilitados, acumuladas desde el inicio de la ocurrencia. Los pasos deshabilitados no ejecutan ni consumen tiempo, pero sus referencias definidas sí se incluyen en la evidencia Tool.

Separar estrictamente routing de presentación: Strategic no dispone de contrato visual Alarm. La sincronización automática de `visual_targets` a partir de routing en la Web debe contrastarse con el texto del canonical sobre independencia entre ambos (CONFLICT DOCUMENTAL/CONTRACTUAL; no corregirlo sin decisión expresa).

## Proyección de configuración y siguiente frontera

**CURRENT:** existen `AlarmConfigurationProjectionBuilder`, codec, stores Local/Cosmos y `compose_alarm_configuration_persistence`. El host de prueba local usa provider local. **UNVERIFIED:** ejecución real integrada Blob/Cosmos y operación continua del productor de la proyección operacional.

**PLANNED / SIGUIENTE FOCO ÚNICO:** proceso/job de Materialization en backend; primero verificar disponibilidad y contrato de la proyección operacional exacta, adquisición de qualification explícita y ownership de stores de salida. Implementar sólo tras fijar esos contratos. No mezclar con Runtime Adoption ni Live Delivery.

## Fronteras ajenas a este incremento

- Backend Core no contiene geometría UI ni scheduler visual.
- Resolver B.2 no hace I/O, adquisición, scheduler, writes ni Runtime Adoption.
- Runtime Adoption y Effective Head siguen posteriores; `READY` no concede automáticamente `EFFECTIVE`.
- Blob es la autoridad durable objetivo en dominios migrados. La proyección Cosmos puede ser superficie operacional de consumo; no es licencia para sustituir la evidencia Tool exacta.
- Proyecto Python `3.14.7`, mientras paquetes Command Center examinados exigen `3.14.2`: **OPEN / SEPARATE**. Las pruebas reportadas usan `uv --python 3.14.2`.
- No introducir adapters de compatibilidad, procesos remotos adicionales, schemas físicos ni defaults sin evidencia/decisión.
