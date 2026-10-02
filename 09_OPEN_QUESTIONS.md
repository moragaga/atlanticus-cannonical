# Atlanticus — Open Questions

Estado: **OPEN ITEMS BY FRONT — POST WEB TOOLING CLEANUP**

## CLOSED

```text
shared Web distribution cleanup
ADA starter runtime thinning
ADA Master Projection ownership migration
Command Center distribution profile
cross-platform locked-sdist wheelhouse fallback
```

No reabrirlos sin finding real.

## OPEN / NEXT — `.env.detail` ADA + Command Center

### ADA

Verificar y congelar:

```text
ATLANTICUS_ENVIRONMENT
ADA_PERSISTENCE_MODE
ADA_APPLICATION_NAMESPACE
ADA_TOOL_NAMESPACE
Blob credential mode
Blob container
Tool Projection Cosmos endpoint/key/database
KPI Delivery Cosmos optional connection
Master Projection derived identity
```

Target operativo siguiente:

```text
environment=local
persistence=durable
Storage=final/durable
Cosmos=local
```

Debe verificarse qué valores pueden derivarse y cuáles deben ser manuales.

### Command Center

Current runtime es local-only.

OPEN:

```text
persistence contract replacement/alignment
durable Manager configuration
Storage final contract
Cosmos local contract
Master Projection integration
production identity remains separate
```

No declarar `ADA_COMMAND_CENTER_COSMOS_*` operativo sólo porque aparece documentado; el launcher CURRENT no activa durable Manager.

## OPEN — Master Projection Command Center

Requirement agreed:

```text
Command Center also needs Master Projection
```

Implementation:

```text
NOT IMPLEMENTED
```

No copiar la implementación ADA dentro del Starter ni hacer Command Center dependiente de `ada-generic-application`.

## OPEN — ADA UI without data

VERIFIED:

```text
ContentStatePresentationMode.AUTHORING
```

suprime overlays visuales degradados.

UNVERIFIED:

```text
all components can render with no KPI/data
```

No existe `ContentState.NO_DATA`.

OPEN para el frente Collector/UI:

- distinguir ausencia inicial de observación de error real;
- decidir si hace falta `NO_DATA` / `WAITING` u otro estado;
- no inventarlo antes de probar Collector y consumidores reales.

## OPEN — ADA KPI/Collector operational E2E

PLANNED después de levantar ambas aplicaciones.

Validar:

```text
KPI data
→ delivery
→ collector
→ Latest / Timeseries stores
→ UI consumption
```

Este frente será exclusivamente ADA.

## Separate

```text
Python 3.14.7 / Trixie migration
Entra production identity
Azure production
Alarm Engine / Analytics
```

No mezclar con el próximo `.env.detail` focus.
