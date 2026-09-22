# ADA Command Center — Open Items

Estado: **OPEN / B.2 NEXT**

Cerrado antes de este checkpoint:
- Tool Catalog V1;
- Alarm Tool References V1;
- structured Alarm Configuration authoring;
- Management suppression por `priority_order`;
- Special Condition Runtime reappearance;
- level-trigger semantics de Special Condition.

## Siguiente foco único

```text
B.2 — Alarm Configuration -> Runtime Materialization Contract
PLANNED / NEXT
```

El próximo chat debe trabajar primero el contrato y la frontera backend.

No implementar consumidores antes de congelar:
- resolution identity/provenance;
- findings;
- Runtime readiness;
- materialización de `PlannedAlarm`;
- materialización de evaluator/parameters;
- adoption.

Delivery debe quedar fuera del primer incremento si mezclarlo impide cerrar un contrato verificable.

## B.2 OPEN

Debe reconciliar:

```text
AlarmConfiguration Projection
+
Tool Catalog revision
+
evaluator availability/contracts
->
resolved/materialized Runtime inputs
```

Sin invalidar retrospectivamente una Alarm Source revision intrínsecamente válida.

### Mapeos Runtime que ya tienen destino CURRENT

B.2 debe poder producir:
- `identity`;
- `kind`;
- `criticality`;
- `priority_group`;
- `priority_order`;
- evaluator key;
- parameters/ExecutionEntry;
- routing C1/C2/C3;
- deactivation policy;
- `reappearance_special_conditions`;
- provenance Runtime.

No inventar nuevos campos si ya existe un contrato Runtime suficiente.

### Special Condition

B.2 conserva la responsabilidad de verificar que las referencias declaradas en authoring sean triggers Special Condition válidos según el contrato vigente.

Engine no necesita transportar `is_special_condition`.

### Execution

`is_active=false`:
- Rule sigue definida;
- no entra a nueva execution session;
- adoption debe cerrar occurrence abierta como configuration-disabled cuando corresponda.

### Visibility

OPEN:

```text
visibility_mode=TRACE_ONLY
!=
PlannedAlarm.delivery_enabled=false
```

Resolver sin alterar priority/Management involuntariamente.

### C2 waits

OPEN para congelación B.2:

```text
wait_minutes_from_previous_step
-> effective cumulative delay_seconds
```

No implementar hasta cerrar el contrato.

### Adoption conflicts

Mantener visibles:
- origin Tool;
- evaluator key;
- kind;
- priority group.

No resolverlos con adapters temporales.

## Otras áreas OPEN pero fuera del siguiente foco

- Message override efectivo con múltiples Messages;
- deactivation effective capability;
- routing tier matrix;
- Rule area vs Tool scope;
- Strategic visual projection;
- presentation terminology;
- application shell final;
- History/Analytics;
- Live/Analytics cadence;
- Golden Path end-to-end.

## Decisions conflict

B.1 frozen Special Cascade todavía difiere de la implementación CURRENT.

Canonical debe mostrar el conflicto hasta que `atlanticus-decisions` registre explícitamente el refinamiento.

## Cross-cutting

Cualquier conflicto de baseline Python existente fuera de este milestone no debe corregirse silenciosamente dentro de B.2.
