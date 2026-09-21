# ADA Command Center — Golden Path

Estado: **PARTIALLY IMPLEMENTED / STRUCTURED AUTHORING NEXT / B.2 LATER**

El Golden Path debe demostrar tanto configuración con Tool disponible como preconfiguración antes de
que una dependencia externa esté disponible.

## Flujo principal y estado

```text
1. Alarm Configuration
   ├── Rule
   ├── evaluator + generic typed parameters
   ├── Message
   ├── deactivation
   ├── escalation
   └── Tool/visual references
                ↓
   CLOSED / VERIFIED / CURRENT

2. Intrinsic validation
                ↓
   CLOSED / VERIFIED / CURRENT

3. Alarm Source/Release + base Projection
                ↓
   CLOSED / VERIFIED / CURRENT contract
   production Blob binding remains OPEN

4. Command Center Tool Catalog V1
   └── consolidated from explicit Tool Projection inputs
                ↓
   CLOSED / CURRENT

5. Alarm Tool Reference read model
   └── Tool → Component → visible Subcomponent options
                ↓
   CLOSED / CURRENT

6. Structured Alarm Configuration authoring UI
                ↓
   NEXT / PLANNED

7. B.2 Resolution
   ├── Runtime readiness
   └── Delivery/reference readiness
                ↓
   PLANNED

8. Alarm Runtime
                ↓
   EXISTING ENGINE / B.2 ADOPTION RECONCILIATION OPEN

9. Durable History
   ├── Occurrence/Episode
   ├── Journey
   └── Evidence
                ↓
   ENGINE FACTS CURRENT / COMMAND CENTER READ MODEL OPEN

10a. Live Projection → ADA Generic
10b. History/Analytics → Command Center
   PLANNED / OPEN
```

## Tool Catalog milestone CURRENT

El catálogo:

- no duplica Tool authoring;
- no introduce Cosmos propio de Command Center;
- consolida sólo cuando todos los inputs configurados son válidos;
- publica un único snapshot CURRENT en Blob;
- conserva `source_release_id` y `ToolStructure` por Tool;
- rechaza `tool_key` duplicado.

## Authoring reference milestone CURRENT

Alarm Configuration ya puede consumir el catálogo mediante un read model backend-only.

La UI estructurada todavía no está implementada.

El read model no altera el contrato durable ni convierte catálogo ausente en configuración inválida.

## Preconfiguration path congelado

Debe seguir siendo válido:

```text
Alarm Source A17
→ references tool_key not yet available
→ A17 remains intrinsically valid and persisted
→ future B.2 reports Tool reference unresolved

later:

Tool Catalog T41
→ referenced Tool becomes available
→ A17 can be resolved again against T41
→ no new Alarm Source release is required
```

## Definition of Done — estado

1. Manager generic sin duplicación — **DONE**;
2. Rules + Messages aggregate — **DONE**;
3. Source/Release contract — **DONE / PRODUCTIVE BINDING OPEN**;
4. parameters `str | float | bool` — **DONE**;
5. Tool Catalog consolida Tools externas sin Cosmos propio — **DONE V1**;
6. inputs múltiples se expresan como `ToolCatalogInput` con stores explícitos — **DONE CONTRACT / PRODUCTIVE COMPOSITION OPEN**;
7. Rule puede persistirse con referencias externas aún no resueltas — **DONE**;
8. backend authoring Tool/Component/Subcomponent — **DONE V1**;
9. structured authoring UI consume ese read model — **NEXT**;
10. B.2 distingue intrinsic validity de external resolution/readiness — **PLANNED**;
11. Runtime adopta configuración ejecutable B.2 — **PLANNED / RECONCILIATION OPEN**;
12. Journey + Evidence — **ENGINE CAPABILITY CURRENT**;
13. Delivery no despacha hacia referencias no resueltas — **FROZEN SEMANTICS / IMPLEMENTATION OPEN**;
14. re-resolution con nueva Tool Catalog revision — **FROZEN SEMANTICS / B.2 OPEN**;
15. Live/History surfaces — **OPEN**.
