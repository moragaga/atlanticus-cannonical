# Alarm Engine — Management

Estado: **CURRENT / RANK SUPPRESSION + DEACTIVATION BARRIER + REAPPEARANCE IMPLEMENTED / TESTED**

Checkpoint:

```text
moragaga/atlanticus:main
cd08bd8d2c25bd89eb39fa15cbda209c8e9be617
```

## Separación semántica

Management representa acciones y efectos operacionales durables.

Deactivation es un effect operacional independiente.

Ninguno redefine estado físico:

```text
managed != physical false
deactivated != physical false
```

## ManagementEffect

Una acción efectiva puede crear:

```text
ManagementEffect(
    effect_id,
    source_occurrence_id,
    effective_at,
    reappearance_due_at,
)
```

La gestión directa aplica a la occurrence identificada; una nueva occurrence no hereda Management
directo de la occurrence anterior.

## DeactivationEffect

CURRENT:

```text
DeactivationEffect(
    effect_id,
    source_occurrence_id,
    effective_from,
    effective_until,
)
```

`source_occurrence_id` queda preservado como provenance del effect.

Una deactivation pendiente de aprobación no es una deactivation efectiva y no suprime.

## Suppression por ranking

CURRENT:

```text
source rank P
target rank < P -> no suppression
target rank > P -> eligible para suppression
```

`kind` no decide source/target eligibility.

El coupling histórico:

```text
IMPACT -> lower RISK
```

queda **SUPERSEDED en implementación**.

Visibility no participa en Core Runtime suppression.

## Dos fuentes de CascadeSuppression

La cascade puede estar sostenida por:

```text
active ManagementEffect
OR
active DeactivationEffect
```

`CascadeSuppression` conserva la causa explícita:

```text
management_effect_id: str | None
deactivation_effect_id: str | None
```

Invariante:

```text
exactly one is non-None
```

Si ambos effects están vigentes, deactivation domina la atribución causal.

### Scope de deactivation

Mientras una deactivation de una source Rule siga vigente:

```text
source -> DEACTIVATED

cada target activo
con target.priority_order > source.priority_order
dentro del mismo priority_group
-> CASCADE_SUPPRESSED
```

Esto permanece cierto aunque su `ManagementEffect` ya haya sido liberado.

Cuando la deactivation expira:
- el effect se limpia;
- desaparece la suppression causada por él;
- priority se recalcula desde el estado físico/operacional vigente.

### Scope de Management

Un ManagementEffect activo mantiene suppression hacia targets activos de menor prioridad
mientras siga dentro de su alcance.

Cuando el effect termina y no existe deactivation vigente que sostenga la cascade:
- la suppression desaparece;
- priority se recalcula.

## Reappearance temporal

Al vencer `reappearance_due_at`, si la occurrence gestionada corresponde al effect y puede reaparecer,
Management puede liberar el effect.

Si no existe deactivation vigente:
- se conserva occurrence;
- incrementa `management_cycle`;
- se emite `ReappearanceChange`.

Si existe deactivation vigente:
- el ManagementEffect puede limpiarse;
- no se atraviesa la deactivation;
- en el camino caracterizado no se emite `ReappearanceChange`;
- la Rule sigue `DEACTIVATED`.

## Reappearance por Special Condition

`PlannedAlarm` transporta:

```text
reappearance_special_conditions: tuple[AlarmIdentity, ...]
```

La semántica es OR y level-triggered.

Cuando una Special Condition dispara mientras no existe una deactivation vigente, puede liberar
Management y producir reappearance según el contrato ya implementado.

Cuando dispara durante una deactivation vigente:
- puede liberar el ManagementEffect;
- no libera la DeactivationEffect;
- no permite que la source ni los targets crucen la barrera operacional de deactivation.

Una referencia:
- `INACTIVE` no dispara;
- `ERROR` no dispara;
- no configurada no dispara.

Una occurrence cerrada no se resucita.

Timer + trigger en el mismo ciclo no duplican reappearance.

## Runtime timer contract

`PlannedAlarm` CURRENT transporta:

```text
reappearance_after_seconds: int | None
```

Core valida tipo/rango, pero la materialización Domain:

```text
ReappearanceDefinition.after_minutes -> reappearance_after_seconds
```

pertenece al pure B.2 resolver.

El cálculo de `reappearance_due_at` sigue entrando al ciclo mediante el resolver/factory Runtime
existente; este hito no reemplazó esa frontera.

## Routing

Management/deactivation suppression no detiene routing.

Se preserva el progreso de routing durante deactivation.

## Special Condition qualification boundary

Engine no transporta ni valida `is_special_condition`.

B.2 debe garantizar que `reappearance_special_conditions` provenga de referencias intrínsecamente
válidas y calificadas según Alarm Configuration.

No duplicar en Core las reglas de authoring.

## Evidencia de este cierre

Sobre el working tree que contenía ambos incrementos del hito:
- suite completa `alarms/core`: `180 passed`;
- `ruff check .`: PASS;
- `ruff format --check .`: PASS;
- 40 archivos del package conformes a Ruff format.

El commit final `cd08bd8...` contiene el incremento `reappearance_after_seconds`;
el exact full gate ejecutado después del commit no quedó capturado y por tanto no se inventa.

La qualification histórica F-010 permanece baseline de persistencia/concurrency/recovery y no fue reabierta.
