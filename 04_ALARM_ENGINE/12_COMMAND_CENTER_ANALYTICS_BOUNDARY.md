# Alarm Engine — Command Center Analytics Boundary

Estado: **CANDIDATE**

Alarm Engine pertenece al backend de ADA Command Center, pero no debe incorporar lógica de dashboard.

Engine produce hechos durables.

Analytics los proyecta para consulta.

```text
Alarm Engine
    ↓ durable facts
History / Analytics read model
    ↓
Command Center Web
```

Fuentes útiles:
- Occurrence/Episode;
- Journey;
- Evidence;
- management/deactivation;
- routing;
- priority transitions;
- configuration/tool revisions.

Mantener separadas:
- Live Projection;
- Management Projection;
- History/Analytics.

Web no lee WAL directo.

Analytics no modifica estado del Engine.
