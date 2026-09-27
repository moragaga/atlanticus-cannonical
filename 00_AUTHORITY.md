# Atlanticus — Authority

Estado: **CURRENT / CHECKPOINT ACOTADO A ALARM MATERIALIZATION + RUNTIME ADOPTION B1 / 2026-09-27**

## Fuentes autoritativas contrastadas

| Fuente | SHA confirmado por lectura de Git | Responsabilidad |
|---|---|---|
| `moragaga/atlanticus:main` | `c8f23d91ae1cb817be55b4b812b22ffca518880e` | Realidad implementada al cierre de B1. |
| `moragaga/atlanticus-cannonical:main` | `58241ddb6db5adbd2e783c7ec9f456f1bda5a321` | Baseline documental **anterior** a la integración de estos reemplazos. |
| `moragaga/atlanticus-decisions:main` | `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e` | Decisiones históricas, contratos y racionales; contrastar vigencia frente a main. |

La comprobación de Git y código fue de **solo lectura**. El alcance de esta auditoría se limita a la frontera de Alarm Materialization y planificación de Runtime Adoption B1: no certifica todo Atlanticus ni infraestructura productiva. Al integrar los reemplazos, el SHA de canonical cambiará; este SHA identifica deliberadamente **la base revisada**, no el futuro HEAD.

Jerarquía operativa:

1. `atlanticus:main`: implementación realmente existente, verificada de nuevo al abrir cada incremento.
2. `atlanticus-cannonical:main`: estado y contratos documentados; contrastar con implementación y registrar diferencias.
3. Tests, qualification y logs: evidencia limitada al commit/árbol y al entorno donde efectivamente se ejecutaron.
4. Decisiones explícitas vigentes del Project, identificando las que todavía no se implementaron.
5. `atlanticus-decisions:main`: genealogía y decisiones; no reintroducir alternativas ya reemplazadas.
6. Conversaciones e historial: contexto de búsqueda, nunca sustituto de las fuentes anteriores.

Ante divergencia entre fuentes: **exponer conflicto**, no resolverlo silenciosamente. Git es **SOLO LECTURA** salvo autorización remota explícita.

## Estado verificado en esta frontera

**CURRENT / VERIFIED en main:** Source Alarm v3 con `AlarmConfigurationSnapshot` y `ToolDependencyManifest` exacto; proyecciones Local/Cosmos existentes; resolver B.2 determinista puro; proceso Materialization con adquisición de proyección Cosmos y publicación de una pareja READY inmutable **en volumen local**, o diagnóstico BLOCKED sin artefactos ejecutables; codec y lector compartidos; lector local exacto de Runtime. El antiguo publicador Cosmos **de salida** y el codec local duplicado ya no aparecen en el árbol actual.

**CURRENT / VERIFIED en main — B1:** `AlarmConfigurationArtifactRef(source_key, result_id, manifest_sha256, resolution_key)`; `AlarmConfigurationRevision` vinculada a esa referencia; constructor desde un `ReadyAlarmMaterialization` previamente leído y un `AlarmEvaluatorRegistry` explícito; planificador sobre unión de identidades definidas de origen/destino; disposiciones `ADDED` y `ENABLED`, además de las anteriores. Es planificación, **no** ejecución completa de esas transiciones.

**CLOSED — validación local comunicada por el usuario:** Incremento A y B1, con pruebas unitarias, Ruff, formatter y construcción de wheels en el árbol de trabajo; los cambios de ambos incrementos están presentes en `atlanticus@c8f23d9`. La comprobación remota confirma implementación, no equivale a una repetición de tests/CI en checkout limpio de ese SHA.

**PLANNED / UNVERIFIED:** consumo de esta referencia como EFFECTIVE global, adopción durable, recuperación cruzada entre WAL del Engine y Effective Head, ejecución segura de `ADDED`/`ENABLED`, lectura posterior de Delivery, qualification operacional y E2E real de Cosmos/Blob. Materialization no concede autoridad EFFECTIVE.

## Invariantes que permanecen congelados

```text
LATEST SAVED = LATEST VALID_AT_SAVE
VALID_AT_SAVE != READY != EFFECTIVE
INVALID != REMOVED
DISABLED != INVALID
DISABLED != REMOVED
TRACE_ONLY != REMOVED
READY != EFFECTIVE
AlarmResolutionKey = (alarm_configuration_revision, confirmed_tool_catalog_revision)
Identidad de artefacto exacto = (source_key, result_id, manifest_sha256, resolution_key)
WAL -> DURABLE HEAD -> SNAPSHOTS -> MATERIALIZED HEAD
```

El `ToolDependencyManifest(Cn)` publicado con Rn no se reconstruye consultando el latest Tool Catalog. Blob/Storage es el destino durable objetivo en dominios ya migrados; Cosmos puede ser proyección de consumo. Materialization adquiere esa proyección Cosmos; Runtime y los futuros consumidores de su configuración B.2 usan el artefacto local exacto, **no** releen Cosmos para reinterpretar la configuración.

El resolver `resolve_alarm_configuration` sigue siendo puro; el **paquete** `backend/alarms/materialization` ahora también alberga codec y lector local compartido con I/O de lectura. Describir al paquete completo como puramente funcional sería inexacto; no trasladar ni duplicar código sin un debate técnico autorizado.

Strict routing sigue congelado: `PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC -> END`, sin saltos/retrocesos/mismo nivel; Strategic es terminal. La divergencia entre independencia conceptual de visual targets y sincronización del editor continúa OPEN.

**Política de versiones predespacho:** los tres paquetes de este frente (`alarms/materialization`, `processes/alarms-materialization`, `processes/alarms-runtime`) permanecen en `1.0.0`. Los avances se identifican por commits/incrementos; conservar las versiones existentes de otras dependencias. Metadatos de Command Center exigen Python `==3.14.2`; objetivo transversal del Project `3.14.7` sigue pendiente en otro frente.

## Frontera siguiente

El próximo **foco técnico único**, después de integrar esta documentación, es **diseñar** la ejecución segura y durabilidad global de Runtime Adoption sobre el WAL existente. Partir de `adoption.py`, `adoption_execution.py`, `job_composition.py`, `persistence/store.py` y contratos B1; resolver primero la brecha `requires_execution_upgrade` frente al ejecutor actual. No implementar automáticamente, no introducir WAL paralelo, grupos sintéticos, compatibilidad legacy ni integrar Delivery dentro del mismo incremento.
