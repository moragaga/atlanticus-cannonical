# Alarm Engine — Open Items

Estado: **CURRENT — 13F.2c Runtime durable CLOSED; se mantienen abiertos solo límites reales**. Baseline: `atlanticus@758249d5fa35236b0ac9b990a393083b4463a507`.

## CLOSED — Runtime durable (13F.2c)

- 13F.2c.3a: recuperación durable de EFFECTIVE, READY exacto, snapshots V3 y technical incidents.
- 13F.2c.3b: commit de ciclo bajo WAL y fencing, memoria posterior a confirmación.
- 13F.2c.3c.1: rebase exclusivo de configuración para grupos sin transición operacional.
- 13F.2c.3c.2: bootstrap READY → EFFECTIVE y adopción durable sin eventos físicos.
- 13F.2c.3c.3: adopción mixta con transiciones de lifecycle, cierre de occurrence/episode y resolución de incidents.
- 13F.2c.3d: auditoría de cobertura cerrada, sin nuevas pruebas redundantes.

El bloqueo legacy por imports de Operational Data removidos corresponde a la antigua implementación `scopes/ada-command-center/backend/processes/alarms-runtime`. La ruta actual `scopes/ada-alarm-engine/processes/alarm-runtime` utiliza contratos de inputs vigentes y está implementada. **No reabrir el bloqueo legacy ni crear shims.**

## OPEN — Qualification e integración productiva

- **UNVERIFIED:** arranque físico de la composición actual con datos reales/controlados, lease y persistencia del nuevo proceso.
- **OPEN:** registro y qualification de evaluadores productivos. El registry actual está vacío (`contracts=()`).
- **UNVERIFIED:** producción de CURRENT/FACTS por la nueva ruta y su conexión downstream. La composición de Runtime actual no incluye el exportador histórico.
- **OPEN / fuera del hito:** qualification/provisioning de infraestructura en Docker/Azure, Key Vault, Entra y escenarios multi-host.
- **OPEN condicional:** inventario/migración de volúmenes históricos incompatibles antes de usar contratos de exportación nuevos.

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

Cerrar la sincronización documental de este hito y escoger **un único frente de qualification/integración**, separado de 13F.2c. No abrir más incrementos de WAL/fencing sin una brecha reproducible.
