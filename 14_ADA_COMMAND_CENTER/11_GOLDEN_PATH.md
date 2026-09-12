# ADA Command Center — Golden Path

Estado: **PROPOSED / FIRST INTEGRATED DELIVERABLE CANDIDATE**

## Flujo

```text
1. Tool Configuration
   └── Tool + Component/Subcomponent
                ↓
2. Source Release / Projection
                ↓
3. Command Center Tool Catalog
                ↓
4. Alarm Configuration
   ├── Rule
   ├── evaluator + parameters
   ├── Message
   ├── deactivation
   ├── escalation
   └── visual target
                ↓
5. B.2 Resolution
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

## Definition of Done candidata

Una Tool real puede:

1. publicarse con topología válida;
2. ser leída por Command Center sin duplicar authoring;
3. usarse para crear una Rule válida;
4. referenciar Message;
5. recibir parámetros válidos;
6. configurar deactivation/escalation/target según el caso;
7. materializar configuración ejecutable;
8. ser adoptada por Alarm Runtime;
9. generar una occurrence controlada/real;
10. producir Journey + Evidence;
11. proyectar estado hacia ADA;
12. aparecer en Command Center con historia reconstruible;
13. producir al menos una conclusión trazable.

Esto demuestra producto y arquitectura en una sola vertical.
