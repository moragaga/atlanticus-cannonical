# ADA Generic — First Tool Delivery Order

Estado: **CURRENT — OPERACIONES INTEGRADAS IS THE ACTIVE REAL TOOL / ROOT CUTOVER NEXT**

Initial delivery order remains:

```text
1. Operaciones Integradas
2. Mina
```

## Operaciones Integradas evidence

A real Tool Projection has been created with:

```text
tool_key      tool_operaciones_integradas_af1b7d9983bd
display_name  Operaciones Integradas
kind          integrated_operations
sources       pi, dispatch
```

KPI Registry was also projected against that exact Tool Projection.

This real configuration surfaced the current ownership gaps before production Storage was used.

## Finding

`conciencia_situacional` is the application-global scope.

`operaciones_integradas` is the Tool scope.

Therefore Tool-varying configuration must not live directly under `conciencia_situacional/sources`.

## Unique next focus

```text
ADA-TOOL-SCOPED-CONFIGURATION-AND-USER-RUNTIME
```

After it is implemented and distribution regenerated:

```text
resume Operaciones Integradas configuration
→ recovery gate
→ UI operational integration
→ Alarm integration in a separate focus
```

Do not open Tool→Tool consolidation semantics or broad multi-tool infrastructure protection during the next increment.
