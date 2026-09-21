# ADA Command Center — Golden Path

Estado: **REFINED / FIRST INTEGRATED DELIVERABLE / IMPLEMENTATION PENDING**

El Golden Path debe demostrar tanto configuración con Tool disponible como preconfiguración antes de que una dependencia externa esté disponible.

## Flujo principal

```text
1. Alarm Configuration
   ├── Rule
   ├── evaluator + generic typed parameters
   ├── Message
   ├── deactivation
   ├── escalation
   └── Tool/visual references
                ↓
2. Intrinsic validation
                ↓
3. Alarm Source Release in Blob
                ↓
4. Command Center Tool Catalog
   └── reconciled from named external Cosmos connections
                ↓
5. B.2 Resolution
   ├── Runtime readiness
   └── Delivery/reference readiness
                ↓
6. Alarm Runtime
                ↓
7. Durable History
   ├── Occurrence/Episode
   ├── Journey
   └── Evidence
                ↓
8a. Live Projection → ADA Generic
8b. History/Analytics → Command Center
```

## Preconfiguration path

Debe ser válido el siguiente caso:

```text
Alarm Source A17
→ references tool_key not yet available
→ A17 remains intrinsically valid and persisted
→ resolution reports Tool reference unresolved
→ eligible Runtime logic may execute
→ Delivery/routing dependent on that Tool does not dispatch

later:

Tool Catalog T41
→ referenced Tool becomes available
→ A17 is resolved again against T41
→ dependent capability becomes READY
→ no new Alarm Source release is required
```

## Definition of Done candidata

La primera vertical integrada debe demostrar:

1. Alarm Configuration se administra mediante la infraestructura genérica Manager sin duplicar el Manager ADA;
2. Rules + Messages se publican como un único aggregate;
3. Source/Release durable usa Blob en el provider objetivo;
4. parámetros permanecen como `str | float | bool` sin schemas específicos por evaluator;
5. Tool Catalog consolida Tools externas sin duplicarlas en un Cosmos de Command Center;
6. múltiples Cosmos se resuelven mediante conexiones nombradas;
7. una Rule puede persistirse con referencias externas aún no resueltas;
8. B.2 distingue intrinsic validity de external resolution/readiness;
9. una Tool disponible puede resolver Component/Subcomponent/target desde topología confirmada;
10. Alarm Runtime adopta configuración ejecutable;
11. una occurrence controlada/real produce Journey + Evidence;
12. Delivery no despacha hacia referencias no resueltas;
13. una revisión posterior de Tool Catalog puede resolver la misma Alarm Source revision sin republicarla;
14. el estado se proyecta hacia ADA;
15. Command Center puede reconstruir historia y provenance de configuración/resolución.
