# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global — FROZEN

```text
uv; no pip normal
contracts before consumers
backend before frontend
clean root cutover
no legacy adapters/shims/aliases
one focus per increment
Git read-only unless explicit authorization
```

## Non-alarm operational presentation — FROZEN

Decisión CURRENT:

```text
configuration/bindings determine what exists
runtime data determines current state/content
```

La ausencia de Delivery no debe producir una aplicación visualmente vacía cuando la Tool ya tiene estructura válida.

## Presentation stores vs polling — FROZEN

Queda SUPERSEDED:

```text
KPI Delivery configured
→ Collector exists
→ stores exist
```

CURRENT:

```text
Tool READY
→ KPI presentation stores exist

KPI Delivery configured
→ Collector polling additionally updates them
```

Tool `UNCONFIGURED` no inventa estructura ni stores.

## Authoring — FROZEN

Queda SUPERSEDED la idea de un camino visual falso o separado para Authoring.

CURRENT:

```text
AUTHORING and NORMAL share real composition
```

Authoring oculta overlays operacionales para permitir diseño del componente real.

## Global Indicator state ownership — FROZEN

Queda SUPERSEDED:

```text
independent ContentState per GI
independent ContentState per actual/plan cell
```

CURRENT:

```text
one ContentState for the complete Global Indicator runtime collection
```

Los KPI que el indicador no necesita no se agregan a la definición.

Los KPI declarados por la definición forman parte del contrato de esa colección.

## Global Indicator generic boundary — FROZEN

Queda SUPERSEDED:

```text
IO owns reusable GI responsive/sizing behavior
```

CURRENT:

```text
ada-web-ui-global-indicator
    owns generic geometry/responsive/sizing

Integrated Operations
    owns MINE/PLANT visibility policy and IO-specific overrides
```

No contaminar el componente genérico con reglas Mina/Planta.

## Alarm visibility exception — FROZEN

No generalizar la regla de UI configurada hacia presencia permanente de alarmas.

```text
AlarmDefinition exists
!= visible alarm exists
```

La visibilidad de alarmas depende de lifecycle y projections.

## Header sizing — PLANNED

No congelar todavía proporciones finales entre:

```text
branding
global indicators
alarm-management
alarm-status
```

La evaluación se realiza cuando las cuatro superficies estén presentes.

## Alarm Engine target — PLANNED / NOT IMPLEMENTED

Dirección acordada para el frente posterior:

```text
migrate operational alarm engine
→ target package identity: ada-alarm-engine
```

Antes de implementar:

```text
audit current packages/contracts
identify fields that do not provide value
compare proposed removals against CURRENT canonical + frozen decisions + Web consumers
freeze resulting contract
then perform clean migration
```

No eliminar campos de `AlarmDefinition` ni shared configuration como efecto lateral de la migración runtime.

No crear compatibility layer sólo para mantener el owner histórico.

## Distributed resources / Operational Data / KPI

Las decisiones CURRENT previas de esos frentes permanecen sin cambio.

## Next single focus in this Web track

```text
ADA-WEB-ALARM-SURFACE-FOUNDATION
```
