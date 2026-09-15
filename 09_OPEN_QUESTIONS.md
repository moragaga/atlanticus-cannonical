# Atlanticus — Open Questions

Estado: **CANONICAL OPEN ITEMS**

Los puntos aquí no reabren contratos ya CLOSED.

## Manager generic core

### CLOSED

Ya no están OPEN:

- dual routing exact/legacy;
- `workflow_service` como lifecycle Manager;
- `ExactSourceReaderWorkflow`;
- `ExactSourcePublicationWorkflow`;
- `ExactSourceHistoryWorkflow`;
- `ExactProjectionWorkflow` como frontera Manager;
- `expected_source_revision`;
- reconstruction revision→`ProjectionTarget`;
- `resolve_exact_source_lifecycle`;
- archivos `exact_*` dentro de Manager;
- schema workspace anterior al cutover.

Estado:

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## OPEN — Navigation consumer

Siguiente foco recomendado:

```text
NAVIGATION-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED / NEXT
```

OPEN:

1. localizar todos los archivos Navigation que consumen Manager;
2. verificar su `ManagerModule`;
3. verificar Source reader/publication/history real;
4. verificar Projection service real;
5. eliminar cualquier dependencia del contrato anterior;
6. ejecutar tests scoped;
7. cerrar Navigation antes de abrir Tools.

## OPEN — Tools consumer

```text
TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED
```

No analizar ni implementar en el chat de Navigation.

## OPEN — KPI Configuration consumer

```text
KPI-CONFIG-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED
```

No analizar ni implementar junto con Tools o KPI Definition.

## OPEN — KPI Definition consumer

```text
KPI-DEFINITION-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED
```

Debe cerrarse como componente independiente.

## OPEN — otros consumers

Después de los cuatro nombres conocidos, realizar una búsqueda final en `atlanticus:main` para detectar otros `ManagerModule` o consumers del contrato anterior.

No inventar consumers por nombres históricos.

## BLOCKED — qualification global

```text
MANAGER-CONSUMER-GLOBAL-QUALIFICATION
BLOCKED
```

Motivo:

- Manager core está GREEN scoped;
- consumers todavía no fueron verificados contra el contrato nuevo;
- full Web/ADA no fue ejecutado en `59fcd3e...`.

## UNVERIFIED

- full Web suite en `59fcd3e...`;
- full ADA suite en `59fcd3e...`;
- Docker E2E;
- CI remoto;
- Python 3.14.7/Trixie global;
- ausencia total de legacy fuera de Manager;
- impacto real del cutover en cada consumer hasta inspeccionarlo.

## Otros open items históricos

Los open items de otros dominios permanecen en sus documentos especializados y no fueron revalidados en este cierre.

## Siguiente foco

```text
NAVIGATION-MANAGER-GENERIC-CONSUMER-CUTOVER
```

No mezclarlo con Tools, KPI Configuration, KPI Definition ni otros frentes.
