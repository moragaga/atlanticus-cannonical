# Atlanticus — Authority

Estado: **CURRENT — corte documental acotado a ADA Command Center / Alarm Engine B2c.5d, 2026-09-28**. Este reemplazo actualiza la frontera de alarmas; no revalida por implicación otras áreas de Atlanticus.

## Fuentes contrastadas en modo READ ONLY

| Repositorio | HEAD leído | Uso |
|---|---|---|
| `moragaga/atlanticus:main` | `a799dc15105d3e037f36ab77129ef0cfa8999013` | Realidad implementada al cierre B2c.5d. |
| `moragaga/atlanticus-decisions:main` | `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e` | Decisiones, intención contractual y genealogía. |
| `moragaga/atlanticus-cannonical:main` | `46877f174513b2475f17b7dc739cd43951fa4ed0` | Base documental examinada **antes** de incorporar estos reemplazos. |

Los HEAD deben releerse en cada chat o incremento. El SHA canonical aquí es **base de comparación**, no un commit futuro de integración. El trabajo de este cierre es documental: no ejecuta tests, no monta infraestructura y no escribe Git.

## Jerarquía y conflictos

1. `atlanticus:main`: evidencia primaria de implementación efectiva.
2. `atlanticus-cannonical:main`: estado/documentos actuales que deben contrastarse con el código y reemplazarse explícitamente cuando quedan desfasados.
3. Qualification, tests y logs identificados con commit/árbol y entorno: sólo respaldan lo que realmente comprobaron.
4. Decisiones vigentes del Project y `atlanticus-decisions:main`: intención contractual, distinguiendo IMPLEMENTED de DECIDED/NOT YET IMPLEMENTED; el historial no convierte un deseo en comportamiento actual.
5. Historial conversacional: pista, nunca autoridad por sí misma.

No reconciliar en silencio conflictos entre código, decisions y cannonical. Etiquetas aplicables: `VERIFIED`, `INFERRED`, `ASSUMED`, `PROPOSED`, `UNVERIFIED` y estados `CURRENT`, `IN PROGRESS`, `PLANNED`, `SUPERSEDED`, `BLOCKED`, `CLOSED`.

**Git es SOLO LECTURA por defecto**. Sin autorización explícita, no hacer commits, push, ramas, PR, issues ni otra mutación. Referencias a otros repositorios o implementaciones no transfieren automáticamente contratos ni decisiones.

## Alarm Engine — frontera del corte

**VERIFIED / CURRENT:** Source v3 con `AlarmConfigurationSnapshot` y `ToolDependencyManifest` exactos; resolver B.2 puro; proceso Materialization que publica READY/BLOCKED local, pareja inmutable Runtime/Delivery y lector exacto. B1 planifica revisión de artefacto exacto. B2a mantiene adopción global durable WAL V1 para cero grupos y V2 para 1..N grupos; V1 **no es legacy**. B2b mantiene Effective Head recuperable y lectura exacta. Los incrementos B2c anteriores enlazan ejecución/adopción y ciclos con sesiones fijadas; B2c.5c añade fuentes operacionales actualmente registradas y la posibilidad de requisitos estáticos/dinámicos por contrato; B2c.5d añade catálogo de evaluación y un ejemplo controlado sin registro productivo automático.

**VERIFIED / CURRENT en `a799dc1`:** `processes/alarms-runtime/.../catalog/registry.py` devuelve `AlarmEvaluatorRegistry(contracts=())`; `catalog/examples/threshold/` contiene el ejemplo y sus espejos en `commented/`. `build_alarm_runtime_process(...)` exige `evaluator_registry` y `source_loader` inyectados y `build_alarm_source_adapter(...)` existe. **NO está acreditado aquí** un bootstrap operacional que combine esas dependencias con datasets reales.

**VERIFIED de logs locales aportados por el usuario:** B2c.5c 436 pruebas de regresión; B2c.5d final 7 específicas, 443 de regresión, Ruff PASS y 40 archivos formateados antes de commit. Se comprobó el HEAD final con los cambios. **UNVERIFIED:** repetición sobre checkout limpio del HEAD final, CI, Cosmos/Blob reales, volumen multi-host y fuentes físicas representativas.

## Invariantes que no deben degradarse

```text
LATEST SAVED = LATEST VALID_AT_SAVE
VALID_AT_SAVE != READY != EFFECTIVE
INVALID != REMOVED
DISABLED != INVALID
DISABLED != REMOVED
TRACE_ONLY != REMOVED
READY != EFFECTIVE
AlarmResolutionKey = (alarm_configuration_revision, confirmed_tool_catalog_revision)
Pin de artefacto = (source_key, result_id, manifest_sha256, resolution_key)
WAL -> DURABLE HEAD -> SNAPSHOTS -> MATERIALIZED HEAD
```

El manifest Tool Cn de una release Rn no se reconstruye desde latest Tool Catalog. El WAL es la autoridad de EFFECTIVE; `effective-head.json` es proyección recuperable. Materialization puede usar Cosmos de entrada, pero publica la salida B.2 local inmutable. Strict routing actual: `PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC -> END`, sin saltos ni mismo nivel. El resolver B.2 es puro; no afirmar que todo el paquete Materialization carece de I/O. No introducir decoders/source v2 o stores de salida Cosmos antiguos.

**Versionado:** paquetes Command Center pertinentes siguen en `1.0.0` y exigen Python `==3.14.2`; el objetivo global de Project `3.14.7` es una discrepancia transversal pendiente, no corregida aquí.

## Frontera siguiente

**PROPOSED / PLANNED:** B2c.6, auditar y diseñar exclusivamente el cableado operativo del proceso Alarm Runtime, reutilizando puertos/entrypoints actuales, registro productivo vacío y lector de fuentes existente. No incorporar por esta vía nuevas alarmas reales, Live Delivery, cambios de reglas de desactivación, nuevas fuentes ni abstracciones no solicitadas. Consultar `04_ALARM_ENGINE/13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md`, `10_OPEN_ITEMS.md` y el traspaso de B2c.5d. La Web sólo admite límite numérico de desactivación y carece de opción `fin del turno`; mantener como OPEN separado.
