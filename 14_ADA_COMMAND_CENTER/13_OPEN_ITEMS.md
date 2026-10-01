# ADA Command Center — Open Items

Estado: **CURRENT — MANAGER CONVERGENCE NEXT / RESOURCE PREPARATION DEFERRED AFTER CONVERGENCE**

Implementation checkpoint:

```text
moragaga/atlanticus@a75465745e188da4765e803595b17acaa55d9306
```

## NEXT único

```text
MANAGER-COMPOSITION-CONVERGENCE-AND-DUAL-PRODUCT-INTEGRATION
PLANNED / NEXT
```

Este frente pertenece al Manager/Web compartido y debe completarse en un mismo chat:

1. auditar/congelar el contrato reusable final de Users, Profiles y Navigation Manager compositions;
2. corregir Navigation Manager únicamente tras decisión explícita de sus divergencias;
3. hacer que ADA Generic consuma la composition/versión Manager común;
4. integrar esa misma autoridad en Command Center;
5. alinear tooling, locks, manifests, generación y distribución de ambos productos;
6. calificar ambos consumidores;
7. cerrar con una sola autoridad/version de Manager.

Autoridad CURRENT observada del package:

```text
atlanticus-web-manager==0.3.18
```

Si el frente requiere bump, el package owner decide la nueva versión y todos los consumidores la adoptan.

No crear versiones propias en cada producto.

## Findings que bloquean el uso inmediato de Navigation Manager reusable

```text
authorization method mismatch
eager ServiceRegistry registration
workflow behavior divergence from ADA
source_key divergence
runtime label/provider API divergence
ADA does not consume generic composition
```

Estado:

```text
NAVIGATION-MANAGER-COMPOSITION
BLOCKED
```

## OPEN después del NEXT

| Elemento | Estado | Motivo |
|---|---|---|
| Resource Preparation + startup gate | PLANNED | Importante para Command Center runtime, pero explícitamente posterior a Manager convergence. |
| Command Center generic/runtime Home | PLANNED | No implementado; no usar el ZIP superseded del chat anterior. |
| Tool Catalog local completamente filesystem | OPEN | Host local todavía depende de Storage para catálogo confirmado. |
| C3 qualification GREEN real | BLOCKED BY DESIGN | Productor/verificadores definitivos no cerrados. |
| C5 technical evidence/env | PLANNED | Owner/key/version finales pendientes. |
| Docker/runtime final Command Center | UNVERIFIED | No hay artifact final integrado. |
| Alarm Source/Projection physical E2E | UNVERIFIED | Contracts existentes no acreditan mismo recurso físico. |
| Live Projection | PLANNED | No implementada. |
| Management Capture/Projection | PLANNED / SEPARATE | No mezclar con Manager config. |
| History/Analytics | PLANNED / SEPARATE | No inferir desde FACTS/CURRENT. |
| END_OF_SHIFT operacional | PLANNED / UNVERIFIED | Falta fuente real de shift end. |
| defectos UX de drafts/validaciones/alerts | OPEN | Findings anteriores, fuera de Manager convergence. |
| Python 3.14.7 / Trixie | BLOCKED / DEFERRED | Sólo reabrir con autorización explícita del usuario. |

## SUPERSEDED

La secuencia anterior:

```text
Resource Preparation NEXT
→ Starter después
→ Users/Profiles/Navigation later
```

queda reemplazada por el NEXT de Manager convergence descrito arriba.

El archivo generado fuera de Git:

```text
ada_command_center_foundation_increment.zip
```

queda `SUPERSEDED / DO NOT APPLY`.

## No abrir durante el próximo frente

```text
Live
Analytics
History
Management Capture
Alarm Engine redesign
new identity system
Python/Trixie migration
new configuration contracts
legacy adapters
```
