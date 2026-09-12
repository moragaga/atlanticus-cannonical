# Alarm Engine — Domain Model

Estado: **DESIGN FROZEN / IMPLEMENTED IN CORE**

Fuente principal:
`alarm_decisions/R3.6M-006B.1-alarm-definition-contract-inventory-DESIGN-FROZEN.md`

## Ownership

Owner histórico del contrato: `ada-command-center-alarms-core==1.0.0`.

Core no debe conocer:

- Cosmos
- SharePoint
- Key Vault
- Dash/Flask
- geometría UI
- sesiones web
- WAL/leases/persistencia física
- configuración ejecutable materializada

## Conceptos

- Rule: alarma configurada.
- AlarmDefinition: definición editable canónica.
- PlannedAlarm: definición resuelta para ejecución.
- Occurrence: activación de una Rule.
- Episode: lifecycle compartido dentro de un `priority_group`.

## Identidad

`AlarmIdentity(family_key, alarm_key)`

- key estable;
- no usar `rule_key` separado;
- `rule_name` editable y único dentro de family;
- `display_name` requerido;
- title estático;
- cause template dinámico.

## Configuración

- kind: RISK / IMPACT.
- criticality: C1 / C2 / C3.
- categorías: Ecology / Productivity / Safety / Costs.
- áreas: Mine / Plant, una o más.
- color semántico: RED / YELLOW.
- evaluator: `evaluator_key` + parámetros simples `str|float|bool`.
- evitar código/listas/nested/None en parámetros.
- enteros numéricos se expresan como float.

## Estado de configuración

Una Rule `inactive` sigue definida pero sale del ejecutable. Si tenía ocurrencia abierta, se cierra por `config-disabled`; no resetear toda la family/group.

## Visibilidad

- VISIBLE
- TRACE_ONLY

TRACE_ONLY se evalúa y deja trazabilidad, pero no se publica como visible.

## Priority

- `priority_group`
- `priority_order` positivo y único en el grupo.

Special Condition es una Rule normal con flag; si está managed y es predominante, puede bloquear las demás activas de misma family+priority_group según contrato.

## Reappearance

`Reappearance(after_minutes, special_conditions)`

Reaparece si:
- la condición principal sigue activa y vence el timer; o
- se activa una Special Condition referenciada.

Un cambio de `after_minutes` recalcula el vencimiento. No confundir reappearance de una SC deactivated con reappearance residual de una Rule normal.
