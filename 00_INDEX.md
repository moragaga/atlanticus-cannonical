# Atlanticus Canonical Context — Index

Estado: **CURRENT — WEB OPERATIONAL COMPOSITION / GLOBAL INDICATOR CLOSED; ALARM WEB SURFACE NEXT IN THIS TRACK**

## Autoridad de este cierre

```text
Implementation        moragaga/atlanticus@d97118d202dc1ea5ef3b0d1c18355d3a330824aa
Canonical pre-replace moragaga/atlanticus-cannonical@af936617ebf4e04157e4ef905dd7bda598c1d05a
Decisions             moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Git                   SOLO LECTURA
```

## Estado por frente

| Ubicación | Estado relevante |
|---|---|
| `01_CURRENT_STATE.md` | Global Indicator/Web operational presentation increment CLOSED; final post-refactor visual smoke remains UNVERIFIED. |
| `02_ARCHITECTURE.md` | Tool structure owns non-alarm presentation existence; KPI Delivery polling no longer owns browser-store existence. |
| `03_DECISIONS_CURRENT.md` | Global Indicator sizing/responsive is generic; IO retains Mina/Planta policy and product-specific overrides. |
| `04_ALARM_ENGINE/` | Alarm domain remains CURRENT; Runtime data integration remains BLOCKED; migration to `ada-alarm-engine` is PLANNED after Web alarm-surface foundation. |
| `07_VALIDATION_BASELINE.md` | Collector, Generic, IO and Global Indicator focused suites GREEN; visual post-refactor smoke still open. |
| `17_DISTRIBUTION_AND_TOOLING/` | Independent parallel front; its prior roadmap is not superseded by this Web closure. |

## Checkpoints

```text
KPI-PRESENTATION-STORES-WITHOUT-DELIVERY        CLOSED / CURRENT
GLOBAL-INDICATOR-COLLECTION-CONTENT-STATE       CLOSED / CURRENT
GLOBAL-INDICATOR-AUTHORING-NORMAL               CLOSED / CURRENT
GLOBAL-INDICATOR-GENERIC-RESPONSIVE             CLOSED / CURRENT
IO-MINE-PLANT-GI-POLICY                         CURRENT
POST-REFACTOR-VISUAL-SMOKE                      UNVERIFIED / NON-BLOCKING
ADA-WEB-ALARM-SURFACE-FOUNDATION                PLANNED / NEXT IN WEB TRACK
ADA-ALARM-ENGINE-MIGRATION                      PLANNED / AFTER WEB FOUNDATION
```

## Siguiente frontera única de este track

```text
ADA-WEB-ALARM-SURFACE-FOUNDATION
```

Objetivo acotado:

```text
create first reusable component/card in the alarms flow
integrate existing alarm-management surface
integrate existing alarm-status surface
exercise the full operational header with branding + GI + alarm-management + alarm-status
freeze the Web consumption boundary for alarms
```

No mezclar todavía:

```text
Alarm Engine migration
Alarm domain field removal
History/Analytics
Distribution/tooling parallel work
Python migration
Azure production qualification
```
