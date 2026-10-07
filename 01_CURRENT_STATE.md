# Atlanticus — Current State

Estado: **CURRENT — WEB OPERATIONAL PRESENTATION / GLOBAL INDICATOR HITO CLOSED**

## Autoridad

```text
Implementation
moragaga/atlanticus@d97118d202dc1ea5ef3b0d1c18355d3a330824aa

Canonical base before replacement
moragaga/atlanticus-cannonical@af936617ebf4e04157e4ef905dd7bda598c1d05a
```

## KPI presentation stores — CURRENT / CLOSED

Una Tool `READY` materializa stores browser-side derivados de `ToolStructure` aunque KPI Delivery/Cosmos no esté configurado.

```text
Tool READY
    ↓
component stores + system destination stores
    ↓
empty presentation snapshot when Delivery is absent
    ↓
runtime callbacks can materialize configured UI
```

KPI Delivery polling queda separado:

```text
Tool READY + KPI Delivery configured
    → Collector + polling updates the same presentation stores

Tool READY + KPI Delivery not configured
    → empty presentation stores remain available

Tool UNCONFIGURED
    → no operational KPI presentation stores are invented
```

Esto elimina la dependencia incorrecta:

```text
no Delivery
→ no stores
→ no UI
```

## Operational presentation invariant — CURRENT

Para superficies operacionales no-alarmas adoptadas por esta arquitectura:

```text
configuration / bindings determine what exists
runtime data determines current state/content
```

La ausencia de datos no debe borrar silenciosamente una composición configurada.

Alarmas permanecen como excepción porque su visibilidad está gobernada por occurrence/episode/projection lifecycle.

## Global Indicator — CURRENT / CLOSED

Versiones CURRENT:

```text
ada-web-ui-global-indicator               0.2.10
ada-generic-application                   0.2.29
ada-integrated-operations-application     0.1.2
```

### Collection ownership

`DashboardGlobalIndicatorBinding` representa:

```text
one Global Indicator definition
+ presentation scopes
```

`DashboardGlobalIndicatorsRuntimeBinding` representa:

```text
tool identity
+ tuple of Global Indicator bindings
+ one ContentState for the complete collection
```

El `ContentState` no pertenece a cada valor ni a cada indicador individual.

### Runtime state

La colección completa se degrada como unidad.

```text
declared CONSTRUCTION
    → collection CONSTRUCTION

declared READY + every configured KPI valid
    → collection READY

declared READY + missing/invalid configured KPI payload
    → collection SOURCE_ERROR
```

Un KPI que no forma parte del catálogo simplemente no forma parte del contrato visual.

### Authoring / Normal

La misma composición y los mismos callbacks se usan en ambos modos.

```text
NORMAL
    → Content State overlay visible when applicable

AUTHORING
    → operational overlay hidden
    → real component composition remains visible for design
```

No existe una UI falsa separada para Authoring.

### Generic vs IO boundary

`ada-web-ui-global-indicator` posee ahora el comportamiento reusable de presentación:

```text
compact label → actual / plan geometry
generic placement sizing
two-column mobile layout
vertical growth in mobile
desktop full-height behavior
generic dividers
responsive typography already owned by the component
```

Integrated Operations conserva sólo su política específica:

```text
catalog
MINE / PLANT scopes
scope metadata
show/hide by current IO presentation
Content State host adaptation to the IO header slot
```

No introducir semántica Mina/Planta en el componente genérico.

## Integrated Operations current authoring catalog

El catálogo actual contiene ocho indicadores de prueba para desarrollar composición y responsive behavior.

```text
Indicator 1 → MINE
Indicator 2 → MINE + PLANT
Indicators 3..8 → PLANT
```

La colección está declarada:

```text
ContentState.CONSTRUCTION
```

Es fixture/product authoring CURRENT, no contrato de negocio definitivo.

## Header — CURRENT / PLANNED

Ya existen las superficies/slots conceptuales:

```text
branding
global indicators
alarm-management
alarm-status
```

Global Indicators ya ejercitan su slot.

El reparto final de espacio del header permanece PLANNED hasta integrar simultáneamente `alarm-management` y `alarm-status`; no optimizar proporciones con superficies faltantes.

## KPI Inspection

KPI Inspection continúa integrado.

El flujo de inspección funcionó durante la validación visual previa al refactor genérico final.

El refactor final no modificó el contrato `data-kpi-inspection-key`, pero el smoke visual posterior al último traslado CSS no fue reportado.

Estado:

```text
implementation/tests       CURRENT
post-refactor visual smoke UNVERIFIED
```

## Mina / Planta

El cambio Mina/Planta funcionó durante la validación visual previa al refactor genérico final.

El refactor final conserva explícitamente las clases y selectores IO responsables del filtrado.

Estado:

```text
implementation             CURRENT
post-refactor visual smoke UNVERIFIED
```

## Qualification observada en este hito

```text
ada-web-kpi-collector
    56 passed
    ruff check PASS
    ruff format --check PASS

ada-generic-application focused increment
    17 passed

ada-generic-application application suite after generic refactor
    37 passed

ada-integrated-operations Global Indicator focused suite
    13 passed
    ruff check focused runtime PASS

ada-web-ui-global-indicator
    30 passed
    ruff check PASS
```

`uv lock --check` pasó para:

```text
ada-web-ui-global-indicator
ada-generic-application
ada-integrated-operations-application
```

## Known unrelated formatting drift

`ada-web-ui-global-indicator` package-wide `ruff format --check .` reportó:

```text
Would reformat: tests/test_presentation.py
```

Ese archivo no fue modificado por el incremento.

No se trató como blocker ni se modificó fuera de scope.

## OPEN

```text
post-refactor visual smoke mobile/desktop
post-refactor Mina/Plant smoke
post-refactor KPI Inspection smoke
full header sizing with branding + GI + alarm-management + alarm-status
first reusable alarm component/card
alarm-management/status integration into the active operational experience
Alarm Engine migration after Web boundary is ready
Python 3.14.7 / Trixie migration
production Azure / Entra qualification
```

## NEXT único de este track

```text
ADA-WEB-ALARM-SURFACE-FOUNDATION
```
