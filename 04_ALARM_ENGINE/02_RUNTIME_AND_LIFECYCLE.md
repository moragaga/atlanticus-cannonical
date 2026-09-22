# Alarm Engine — Runtime and Lifecycle

Estado: **CURRENT / IMPLEMENTED / TESTED**

## Cycle boundary

`reduce_group_cycle` recibe:
- estado durable del grupo;
- `cycle_at`;
- `PlannedAlarm`;
- evaluaciones del ciclo;
- closures de configuración;
- acciones de Management;
- decisiones/intentos de deactivation;
- factories/resolvers explícitos.

No usa estado global.

## Orden lógico CURRENT

El ciclo realiza conceptualmente:

```text
1. validar cycle/configuration inputs
2. indexar PlannedAlarm y evaluations
3. preparar Management/deactivation inputs
4. resolver lifecycle físico/técnico y occurrences
5. construir next GroupLifecycleState
6. finalizar Management
   - timers
   - Special Condition reappearance
   - scope cleanup
   - CascadeSuppression
7. resolver routing
8. resolver priority
9. emitir GroupLifecycleDecision
```

La Special Condition reappearance se ejecuta después de resolver las evaluaciones/occurrences del ciclo y antes de routing/priority final.

## Estado físico vs Management

Management no redefine la condición física.

Una occurrence gestionada puede seguir abierta y evaluada ACTIVE.

`CascadeSuppression` modifica disposición operacional, no lifecycle físico.

## Management suppression

CURRENT:

```text
lower priority_order = higher priority
managed P suppresses eligible active targets with priority_order > P
```

No existe coupling Runtime `IMPACT -> RISK` para construir la suppression.

Una Rule de prioridad superior a la fuente gestionada puede emerger.

Routing continúa mientras una Rule está managed, eclipsed o cascade-suppressed según el contrato de routing aplicable.

## Special Condition reappearance

El ciclo deriva el conjunto de identidades evaluadas `ACTIVE`.

Para una Rule gestionada, una identidad configurada en:

```text
PlannedAlarm.reappearance_special_conditions
```

puede liberar el ManagementEffect cuando:
- la Rule principal mantiene occurrence abierta;
- el ManagementEffect corresponde a esa occurrence;
- la Rule principal está evaluada ACTIVE en el ciclo;
- al menos un trigger referenciado está evaluado ACTIVE.

No requiere transición `INACTIVE -> ACTIVE`.

No depende del resultado final de priority.

No resucita occurrences cerradas.

## Priority

Priority se resuelve después de Management finalization.

El candidato predominante se elige por menor `priority_order` entre candidatos operacionales.

Disposiciones CURRENT incluyen:
- `PREDOMINANT`;
- `ECLIPSED`;
- `CASCADE_SUPPRESSED`;
- `DEACTIVATED`;
- `SHADOW`.

`delivery_enabled=false` sigue participando en semántica histórica `SHADOW`; no mapear todavía `TRACE_ONLY` a ese flag.

## Reconfiguration/adoption

El Engine ya soporta reconciliation/adoption, pero existen diferencias entre B.1 deseado y comportamiento CURRENT para determinados cambios estructurales.

No resolver esas diferencias dentro de B.2 silenciosamente.
