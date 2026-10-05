# Atlanticus — Open Questions

Estado: **CURRENT — DISTRIBUTION/TOOLING NEXT; ALARM RUNTIME MIGRATION SEPARATE**

## CLOSED

```text
neutral DataInputSpec consumer contract
input identity by input_key
physical consolidation by source/view
DataViewBinding registry normalization
Operational Data legacy contract removal
KPI Core/Evaluation/Runtime migration
focused local qualification
```

## OPEN / NEXT — Distribution and Tooling

El próximo chat debe resolver exclusivamente:

```text
1. Qué artifacts debe generar CURRENT main.
2. Si todos los artifacts se generan correctamente.
3. Si su contenido y dependencias son instalables/consumibles.
4. Qué significa cada campo de .env.detail.
5. Qué campos son required/optional.
6. Qué campos pueden tener default seguro.
7. Qué campos pueden ser system-derived/system-assigned.
8. Qué campos son secretos.
9. Cómo generar la distribución final.
10. Cómo calificar un consumidor aislado de esa distribución.
```

## OPEN / SEPARATE — Alarm Runtime

```text
new Alarm evaluator/input contract
migration from removed DataRequirement path
DataInputSpec declaration
DataInputContext consumption
planner/loader integration
focused qualification
```

No es parte del siguiente incremento.

## OPEN / SEPARATE — Platform

```text
Python 3.14.7/Trixie migration
production Azure/Entra qualification
remaining product-specific fronts already tracked by their canonical sections
```
