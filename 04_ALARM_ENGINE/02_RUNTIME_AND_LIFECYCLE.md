# Alarm Engine — Runtime and Lifecycle

Estado: **CURRENT / IMPLEMENTED / TESTED**

Checkpoint:

```text
moragaga/atlanticus:main
cd08bd8d2c25bd89eb39fa15cbda209c8e9be617
```

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
6. finalizar Management/deactivation
   - timers
   - Special Condition reappearance
   - deactivation expiry
   - scope cleanup
   - CascadeSuppression
7. resolver routing
8. resolver priority
9. emitir GroupLifecycleDecision
```

La Special Condition reappearance se evalúa antes de routing/priority final.

## Estado físico vs Management/deactivation

Management y deactivation no redefinen la condición física.

Una occurrence puede seguir abierta y evaluada `ACTIVE` mientras está gestionada o deactivated.

`CascadeSuppression` modifica disposición operacional, no lifecycle físico.

## Cascade suppression CURRENT

Existen dos fuentes causales independientes:

```text
active ManagementEffect
OR
active DeactivationEffect
```

Scope por ranking:

```text
lower priority_order = higher priority

source rank P
target rank < P -> no suppression
target rank > P -> eligible para suppression
```

Sólo targets activos del mismo `priority_group` son elegibles.

`kind` y visibility no deciden source/target eligibility.

Si source posee simultáneamente ManagementEffect y DeactivationEffect vigentes,
deactivation domina la atribución causal de `CascadeSuppression`.

Contrato de provenance:

```text
CascadeSuppression
    management_effect_id XOR deactivation_effect_id
```

exactamente uno debe existir.

## Deactivation barrier CURRENT

Mientras un `DeactivationEffect` está vigente:

```text
source -> DEACTIVATED
lower-priority active targets -> CASCADE_SUPPRESSED
```

La barrera dura hasta `effective_until`, independientemente de que el ManagementEffect haya sido
liberado por timer o Special Condition.

Pending approval no crea suppression; la suppression comienza al existir un DeactivationEffect efectivo.

Cuando la deactivation expira:
- se limpia el effect;
- la condición física vigente se conserva;
- priority se recalcula desde el estado actual.

El effect puede sobrevivir al cierre de la occurrence/episode y gobernar una occurrence posterior
mientras siga vigente.

## Reappearance temporal y Special Condition durante deactivation

Management puede finalizar por:
- vencimiento temporal;
- Special Condition configurada.

Si esto ocurre mientras la source Rule sigue bajo deactivation:
- el `ManagementEffect` puede limpiarse;
- la deactivation permanece;
- no se atraviesa la barrera operacional;
- no se emite `ReappearanceChange` en el camino caracterizado;
- la source continúa `DEACTIVATED`;
- los targets elegibles continúan `CASCADE_SUPPRESSED`.

Frase congelada:

```text
Una deactivation aplicada a una Rule mantiene suprimidas todas las Rules activas
de menor prioridad dentro del mismo scope de cascada durante toda la vigencia
de la deactivation. Una Special Condition puede disparar reappearance respecto
del management state, pero nunca atraviesa una deactivation vigente.
```

## Routing

Routing continúa mientras una Rule está managed, eclipsed, cascade-suppressed o deactivated
según el contrato de routing aplicable.

Deactivation no pausa por sí misma el progreso C2.

## Priority CURRENT

El candidato predominante se elige por menor `priority_order` entre candidatos operacionales.

Disposiciones CURRENT:

```text
PREDOMINANT
ECLIPSED
CASCADE_SUPPRESSED
DEACTIVATED
```

No existen:

```text
SHADOW
delivery_enabled
```

en Runtime Core.

## Reconfiguration/adoption

El Engine soporta primitives de reconciliation/reset, pero Runtime Adoption global y Effective Head
siguen sin implementar.

La reconciliación de cambios en:
- `reappearance_after_seconds`;
- `reappearance_special_conditions`;

sobre un ManagementEffect/hot state vigente pertenece a Runtime Adoption y sigue OPEN.

No resolver esas diferencias dentro de B.2 silenciosamente.
