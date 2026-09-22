# Alarm Engine — Configuration and Materialization

Estado: **CURRENT SEMANTICS / SOURCE + BASE PROJECTION + TOOL CATALOG CURRENT / RUNTIME CONTRACT EXTENDED / B.2 NEXT**

## Invariante central

```text
LATEST SAVED = LATEST INTRINSICALLY VALID
VALID != FULLY RESOLVED != READY
```

La inexistencia o drift de Tool/evaluator no vuelve retrospectivamente inválida una Alarm Source revision intrínsecamente válida.

## CURRENT antes de B.2

Alarm Configuration ya dispone de:
- aggregate durable Rules + Messages;
- intrinsic validation;
- Source/Release;
- base Projection exacta;
- Manager/history;
- structured authoring;
- Tool Catalog V1;
- Alarm Tool Reference read model.

External resolution/materialization B.2 todavía no existe como owner concreto.

## Runtime contract CURRENT

Alarm Engine recibe `PlannedAlarm`.

Después de este milestone, `PlannedAlarm` incluye:

```text
reappearance_special_conditions: tuple[AlarmIdentity, ...]
```

además de los campos históricos de execution/routing/deactivation/provenance.

Esto resuelve el contrato Runtime necesario para Special Condition reappearance, pero **no implementa B.2**.

No existe todavía un componente que materialice automáticamente:

```text
AlarmDefinition.reappearance.special_conditions
->
PlannedAlarm.reappearance_special_conditions
```

desde una resolución B.2.

## B.2 boundary

B.2 debe resolver una revisión concreta de Alarm Configuration contra dependencias externas concretas y producir contratos consumibles por Runtime/Delivery sin reimplementar lógica del Engine.

El siguiente trabajo debe partir de autoridad y definir antes de consumidores:
- identidad/provenance de resolución;
- findings;
- Runtime readiness;
- Delivery/reference readiness;
- materialización de `PlannedAlarm`;
- materialización de `AlarmExecutionEntry`/parameters donde corresponda;
- adoption de la resolución.

Runtime adoption continúa siendo la autoridad para EFFECTIVE.

Delivery no puede liderar Runtime.

## Special Condition mapping requerido

B.2 debe transformar referencias authoring:

```text
AlarmDefinition.reappearance.special_conditions
```

a:

```text
PlannedAlarm.reappearance_special_conditions
```

sólo después de aplicar las validaciones/qualification que correspondan.

Engine no requiere `is_special_condition`.

B.2 conserva responsabilidad de validar que una referencia declarada como trigger corresponda al contrato Special Condition vigente, incluido su scope cuando así lo exija B.1.

## Visibility conflict aún OPEN

AlarmDefinition:

```text
visibility_mode=TRACE_ONLY
```

significa evaluar + trazar + no publicar visiblemente.

Engine CURRENT:

```text
delivery_enabled=false
```

produce semántica `SHADOW` y afecta priority/Management.

Por tanto:

```text
TRACE_ONLY != delivery_enabled=false
```

B.2 no debe mapearlos ciegamente.

## Execution

`is_active=false` mantiene la Rule definida pero fuera de la nueva execution session.

Adoption debe poder cerrar occurrence abierta por configuration-disabled sin resetear innecesariamente todo el grupo.

La representación materializada necesaria para adoption debe distinguirse de la execution session efectiva.

## Routing

C1/C2/C3 Engine CURRENT permanece:
- C1: origin + destinos configurados inmediatos;
- C2: origin inmediato + destinos retardados;
- C3: origin only.

AlarmDefinition authoring usa waits relativos por step. La transformación futura hacia delays absolutos acumulados para C2 sigue **PROPOSED / B.2 OPEN** hasta congelarse expresamente.

## Adoption conflicts que B.2 no debe ocultar

Persisten diferencias entre B.1 y Engine CURRENT para:
- `origin_tool_key`;
- `evaluator_key`;
- `kind`;
- `priority_group`.

No introducir adapters legacy ni doble contrato para esconderlas.

## Provenance histórico

Runtime todavía contiene revision strings históricos como:

```text
alarm_configuration_revision
tool_registry_revision
```

B.2 debe reconciliarlos limpiamente con la resolución/provenance elegida.

No implementar compatibilidad paralela permanente.

## No reabrir Engine

Management suppression y Special Condition Runtime reappearance están CLOSED.

B.2 debe consumir esos contratos; no rediseñarlos por conveniencia de materialización.
