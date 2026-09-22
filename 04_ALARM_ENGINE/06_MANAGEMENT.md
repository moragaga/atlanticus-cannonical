# Alarm Engine — Management

Estado: **CURRENT / RANK SUPPRESSION + SPECIAL-CONDITION REAPPEARANCE IMPLEMENTED / TESTED**

Checkpoint:

```text
moragaga/atlanticus:main
ebf736a1cf5193a297fbafc55c5c11ca9993f24c
```

## Separación semántica

Management representa acciones y efectos operacionales durables.

No es la fuente de estado físico.

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

La gestión directa aplica a la occurrence identificada; una nueva occurrence no hereda Management directo de la occurrence anterior.

## Suppression por ranking

CURRENT:

```text
source managed rank P
target rank < P -> no suppression
target rank > P -> eligible para suppression
```

`kind` no decide source/target eligibility.

El coupling histórico:

```text
IMPACT -> lower RISK
```

queda **SUPERSEDED en implementación**.

El `delivery_enabled` histórico todavía participa en la elegibilidad de Management y priority. La reconciliación con `TRACE_ONLY` sigue OPEN para B.2/Delivery.

### Scope

Un ManagementEffect puede conservar alcance después del cierre de su source occurrence mientras existan targets elegibles de menor prioridad dentro del episodio y antes de su expiry, según las reglas CURRENT.

Cuando el efecto deja de tener alcance:
- se limpia;
- la suppression desaparece;
- priority se recalcula.

### Routing

Management suppression no detiene routing.

Esto fue caracterizado explícitamente antes del delta productivo y preservado por la suite completa.

## Reappearance temporal

Al vencer `reappearance_due_at`, si la occurrence gestionada corresponde al efecto y puede reaparecer:
- se limpia el efecto;
- se conserva occurrence;
- incrementa `management_cycle`;
- se emite `ReappearanceChange`.

## Reappearance por Special Condition

`PlannedAlarm` transporta:

```text
reappearance_special_conditions: tuple[AlarmIdentity, ...]
```

La finalización de Management recibe las identidades evaluadas `ACTIVE` en el ciclo.

Si:
- la Rule gestionada sigue ACTIVE;
- mantiene occurrence abierta;
- el ManagementEffect corresponde a esa occurrence;
- al menos una referencia configurada está ACTIVE;

entonces:
- se limpia ManagementEffect;
- se conserva occurrence;
- incrementa `management_cycle`;
- se emite una única `ReappearanceChange`.

La semántica es OR y level-triggered.

Una referencia:
- `INACTIVE` no dispara;
- `ERROR` no dispara;
- no configurada no dispara.

Una occurrence cerrada no se resucita.

Timer + trigger en el mismo ciclo no duplican reappearance.

Si el trigger ya estaba ACTIVE al momento de gestionar:
- `ManagementActionOutcome` permanece `EFFECTIVE`;
- existe trazabilidad `STARTED` y `CLEARED` en el mismo ciclo;
- el efecto no queda vigente;
- la misma occurrence reaparece con `management_cycle + 1`.

## Special Condition qualification boundary

Engine no transporta ni valida `is_special_condition`.

B.2 debe garantizar que `reappearance_special_conditions` provenga de referencias intrínsecamente válidas y calificadas según Alarm Configuration.

No duplicar en Core las reglas de authoring.

## Evidencia

Durante este milestone se verificó:
- characterization de Management suppression: 6 PASS después del delta;
- characterization final de Special Condition reappearance: 9 PASS;
- suite completa `alarms/core`: PASS;
- `ruff check .`: PASS;
- format checks de archivos modificados: PASS.

La qualification histórica F-010 permanece baseline de persistencia/concurrency/recovery y no fue reabierta.
