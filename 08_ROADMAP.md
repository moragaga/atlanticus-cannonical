# Atlanticus — Roadmap

Estado: **CANONICAL BASELINE 1.0 — EXECUTION IN PROGRESS**

## Regla

No expandir arquitectura general sin necesidad de producto.

Cerrar verticalmente capacidades integrables y verificables.

Mantener un foco por incremento.

Para la fase actual del Manager:

```text
UN CHAT = UN CONSUMER = UN INCREMENTO CERRABLE
```

## Checkpoint actual de este cierre

```text
moragaga/atlanticus@59fcd3ecc8f3441e64fbe0fc892b4467fa56f181
```

## Hito cerrado

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Resultado:

- una sola ruta Source/Projection;
- legacy Manager removido;
- adapters/shims/aliases de transición prohibidos;
- `expected_source_revision` removido;
- reconstruction revision→`ProjectionTarget` removida;
- workspace genérico basado en `SourceSnapshot`;
- Manager tests: `54 passed`.

## Secuencia de consumers

Cada componente debe cerrarse en chat separado.

Orden recomendado:

```text
1. NAVIGATION-MANAGER-GENERIC-CONSUMER-CUTOVER
2. TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER
3. KPI-CONFIG-MANAGER-GENERIC-CONSUMER-CUTOVER
4. KPI-DEFINITION-MANAGER-GENERIC-CONSUMER-CUTOVER
5. otros consumers reales encontrados en atlanticus:main
6. MANAGER-CONSUMER-GLOBAL-QUALIFICATION
```

No se presupone que todos requieran los mismos cambios.

## Criterio de cierre por consumer

Cada chat debe:

1. localizar su implementación real en `atlanticus:main`;
2. verificar cómo construye `ManagerModule`;
3. verificar sus servicios Source/Projection;
4. reemplazar directamente cualquier contrato anterior;
5. eliminar adapters/shims/aliases;
6. actualizar tests del comportamiento final;
7. ejecutar qualification scoped;
8. cerrar antes de abrir otro consumer.

## No mezclar

Durante cada consumer cutover no mezclar:

- otro consumer de Manager;
- Python migration;
- runtime provenance no relacionado;
- Root/bootstrap físico;
- legacy deletion global de otros dominios;
- Docker E2E general;
- rediseños visuales no contractuales.

## Qualification global

```text
MANAGER-CONSUMER-GLOBAL-QUALIFICATION
BLOCKED
```

Se desbloquea sólo cuando los consumers identificados estén cerrados.

## Otros frentes

Los demás frentes del roadmap anterior continúan según sus documentos especializados y no fueron revalidados por este cierre.
