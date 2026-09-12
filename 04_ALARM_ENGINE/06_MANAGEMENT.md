# Alarm Engine — Management

Estado: **CURRENT**

## Separación semántica

Management representa acciones/estado operacional durable de gestión.

No es la fuente del estado físico Live.

## Consecuencias

- `managed=true` no significa condición física false.
- `deactivated=true` no significa necesariamente que desaparezca del Live mientras la condición física siga activa.
- Management Projection no recalcula prioridad actual.
- Live Projection no debe inferir acciones de management inexistentes.

## Web

Web presenta capacidades/estado ya resuelto; no reimplementa reglas de management del dominio.

## Evidencia

La qualification final F-010 cerró con:
- 480/480 management requests;
- 480/480 management decisions;
- sin pérdidas ni duplicaciones en el cierre recuperado.
