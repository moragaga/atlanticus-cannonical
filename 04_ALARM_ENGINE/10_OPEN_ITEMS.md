# Alarm Engine — Open Items

Estado: **CURRENT — publicación/maintenance del nuevo Alarm Runtime implementadas y qualification sintética local CLOSED; permanecen límites productivos y footprint**. Baseline: `atlanticus@c3b8ed3b8de4bbafdaeeff4410d4daaa20bed1b4` (2026-10-10).

## CLOSED — Runtime durable (13F.2c)

- 13F.2c.3a: recuperación durable de EFFECTIVE, READY exacto, snapshots V3 y technical incidents.
- 13F.2c.3b: commit de ciclo bajo WAL y fencing, memoria posterior a confirmación.
- 13F.2c.3c.1: rebase exclusivo de configuración para grupos sin transición operacional.
- 13F.2c.3c.2: bootstrap READY → EFFECTIVE y adopción durable sin eventos físicos.
- 13F.2c.3c.3: adopción mixta con transiciones de lifecycle, cierre de occurrence/episode y resolución de incidents.
- 13F.2c.3d: auditoría de cobertura cerrada, sin nuevas pruebas redundantes.

El bloqueo legacy por imports de Operational Data removidos corresponde a la antigua implementación `scopes/ada-command-center/backend/processes/alarms-runtime`. La ruta actual `scopes/ada-alarm-engine/processes/alarm-runtime` utiliza contratos de inputs vigentes y está implementada. **No reabrir el bloqueo legacy ni crear shims.**

## CLOSED — incremento de publicación y mantenimiento (2026-10-10)

- Publicación derivada CURRENT durable v1 y FACTS stream/cursor v4 desde el nuevo Alarm Runtime.
- Rotación de WAL, checkpoint dual, replay incremental y compactación protegida por autoridad/fencing.
- Política configurable de resúmenes de iteración en backend/runtime con valor previo por defecto.
- Suites locales 553 PASS y Acceptance Gate v3 sintético 7/7 readjudicado; no implica qualification productiva.

## OPEN — Qualification e integración productiva

- **UNVERIFIED:** arranque físico con evaluadores productivos y datos representativos; el escenario sintético no equivale a esta prueba.
- **OPEN:** registro y qualification de evaluadores productivos. El registry actual está vacío (`contracts=()`).
- **UNVERIFIED:** compatibilidad/consumo físico del nuevo CURRENT durable v1 y FACTS v4 por Modeler/Delivery y superficies downstream; el publicador nuevo ya está integrado.
- **OPEN / fuera del hito:** qualification/provisioning de infraestructura en Docker/Azure, Key Vault, Entra y escenarios multi-host.
- **OPEN condicional:** inventario/migración de volúmenes históricos incompatibles antes de usar contratos de exportación nuevos.
- **OPEN / siguiente foco:** cuantificar y reducir footprint de FACTS; definir retención/archivo sin romper continuidad, cursores o recovery. WAL compactado no implica facts compactados.

## OPEN — Modeler y Delivery (frentes separados)

```text
CAROUSEL full scheduler
QUEUE_IN_QUEUE full scheduler
rotation window
QIQ fairness
durable ModelerState/checkpoint
scheduler recovery
artifact A → B state migration
stale/disconnection policy
Runtime → Modeler ordered/no-drop handoff si llega a requerirse
Modeler → Delivery checkpoint/retry por destino
```

Estos temas no bloquean el cierre de persistencia/adopción del Runtime.

## OPEN — Presentación y negocio

```text
effective dynamic cause
Management projections
History/Analytics projections
Web live rendering y alarm-management
```

La Web consume proyecciones según los límites congelados: no lee WAL como interfaz de visualización y no reconstruye lifecycle.

## HISTORICAL — evidencia de Command Center

La generación anterior verificó localmente READY/EFFECTIVE, CURRENT v1, FACTS v2, Modeler per-Tool, Delivery y read-back Cosmos. Esa evidencia es válida en su época, pero no cualifica automáticamente la nueva composición `ada-alarm-engine`.

## Siguiente foco recomendado

Siguiente incremento propuesto: **Storage Footprint & Retention Qualification** del nuevo FACTS v4, con medición reproducible de volúmenes, reglas de retención y prueba de continuidad. No mezclar con Modeler/Delivery, Web o evaluadores productivos.
