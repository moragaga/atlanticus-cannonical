# Alarm Engine — Projection and Publication

Estado: **DECISION RECORDED**

## Live Projection != Management Projection

Son proyecciones distintas y no deben fusionarse.

Una alarma físicamente activa puede seguir en Live aunque:
- haya sido managed;
- haya sido deactivated administrativamente.

Management state no convierte por sí solo la condición física en false.

## Priority

Priority se resuelve antes de Live Projection.

Web no vuelve a decidir predominancia.

## Occurrence

Una misma occurrence puede alimentar varias superficies sin duplicar identidad.

## Live payload

Debe llegar suficientemente resuelto para Web, incluyendo según contrato:
- identity;
- status/priority;
- title/display/cause;
- kind/criticality/category/areas;
- color semantic;
- visual targets;
- active messages;
- management/deactivation;
- capabilities.

Web no debe reconstruir AlarmDefinition ni catálogos para decidir qué significa la alarma.

## Runtime / Delivery projections

Increment 1 congeló:
- una resolución validada produce Runtime Config Projection;
- produce Delivery Config Projection;
- ambas comparten `resolution_key`;
- Delivery no puede adelantarse a Runtime;
- Runtime adoption determina `EFFECTIVE`.

## Storage

Los documentos originales describen SharePoint como authority/history. Esa parte es binding histórico y debe reconciliarse con Blob Storage. Las invariantes anteriores permanecen hasta decisión explícita en contrario.
