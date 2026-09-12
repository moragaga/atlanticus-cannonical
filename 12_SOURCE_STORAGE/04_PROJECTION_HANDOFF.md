# Source Storage — Projection Handoff

Estado: **DECIDED DIRECTION / NOT YET IMPLEMENTED**

Projection siempre debe trabajar sobre un release Source concreto.

No leer un `latest` mutable durante la proyección.

## Estado observable

Ejemplo:

- Source current = V48
- Projection = V47

Estado:
`SOURCE_AHEAD_OF_PROJECTION`

Acción:
`Project(V48)`

## Fallo de Projection

Si Projection falla:
- Source release queda intacto;
- puede reintentarse;
- no se reescribe historial.

## Metadata

Projection Store debe identificar inequívocamente el `source_release_id` que representa.

Este campo/contrato todavía debe congelarse en la implementación actual.
