# ADA Command Center — Golden Path

Estado: **PARTIALLY IMPLEMENTED / NEXT TOOL CATALOG**

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

4. Command Center Tool Catalog
   └── reconciled from named external Tool projections
                ↓
   NEXT / DESIGN FIRST / NOT YET IMPLEMENTED

5. B.2 Resolution
   ├── Runtime readiness
   └── Delivery/reference readiness
                ↓
   PLANNED

6. Alarm Runtime
                ↓
   EXISTING ENGINE / B.2 ADOPTION RECONCILIATION OPEN

7. Durable History
   ├── Occurrence/Episode
   ├── Journey
   └── Evidence
                ↓
   ENGINE FACTS CURRENT / COMMAND CENTER READ MODEL OPEN

8a. Live Projection → ADA Generic
8b. History/Analytics → Command Center
   PLANNED / OPEN
```

## Manager milestone CURRENT

Alarm Configuration ya se administra mediante `atlanticus.web.manager` sin duplicar el Manager ADA.

Implementado:

```text
workspace
→ validate
→ verify Source
→ publish
→ project
→ history/preview
```

La capability Web específica está bajo ownership de Command Center y actualmente usa un editor
Document mode sobre el mismo aggregate durable.

## Preconfiguration path

Debe seguir siendo válido el siguiente caso:

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

El Tool Catalog necesario para demostrar esta vertical todavía no está implementado.

## Definition of Done candidata

Estado de los criterios:

1. Alarm Configuration se administra mediante infraestructura genérica Manager sin duplicar el Manager ADA — **DONE**;
2. Rules + Messages se publican como un único aggregate — **DONE**;
3. Source/Release adopta el contrato durable Atlanticus y Blob como provider objetivo — **CONTRACT DONE / PRODUCTIVE BINDING OPEN**;
4. parámetros permanecen `str | float | bool` sin schemas específicos por evaluator — **DONE**;
5. Tool Catalog consolida Tools externas sin duplicarlas en un Cosmos de Command Center — **OPEN / NEXT**;
6. múltiples Cosmos se resuelven mediante conexiones nombradas — **OPEN / TOOL CATALOG**;
7. una Rule puede persistirse con referencias externas aún no resueltas — **DONE en intrinsic contract**;
8. B.2 distingue intrinsic validity de external resolution/readiness — **PLANNED**;
9. una Tool disponible puede resolver Component/Subcomponent/target desde topología confirmada — **PLANNED**;
10. Alarm Runtime adopta configuración ejecutable B.2 — **PLANNED / RECONCILIATION OPEN**;
11. una occurrence controlada/real produce Journey + Evidence — **ENGINE CAPABILITY CURRENT**;
12. Delivery no despacha hacia referencias no resueltas — **FROZEN SEMANTICS / IMPLEMENTATION OPEN**;
13. una revisión posterior de Tool Catalog puede resolver la misma Alarm Source revision sin republicarla — **FROZEN SEMANTICS / IMPLEMENTATION OPEN**;
14. el estado se proyecta hacia ADA — **OPEN**;
15. Command Center puede reconstruir historia y provenance de configuración/resolución — **OPEN**.
