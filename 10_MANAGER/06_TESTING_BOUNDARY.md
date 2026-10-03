# Manager — Testing Boundary

Estado: **CURRENT POLICY**

## Principio

Los tests automatizados protegen comportamiento, contratos funcionales, invariantes, regresiones y flujos críticos.

No deben congelar presentación accidental, packaging ni implementación interna.

## Automatizar

Cuando corresponda:

- authorization behavior;
- registry/routing behavior;
- lifecycle;
- draft/save/validate/publish/project transitions;
- conflict/recovery behavior;
- callbacks funcionales;
- persistencia;
- concurrencia real;
- integridad referencial;
- paginación funcional;
- errores y fallbacks;
- integración entre capabilities;
- disponibilidad/carga de un asset sólo cuando sea requisito funcional explícito.

## No crear ni conservar tests cuyo objetivo sea validar

- versiones de packages o `__version__`;
- `requires-python`;
- lista u orden exacto de dependencies de `pyproject.toml`;
- contenido/metadata exacta de wheel/sdist;
- imports permitidos/prohibidos como arquitectura;
- public API shape por existencia/ausencia de símbolos;
- existencia/no existencia de funciones o clases;
- `__all__`;
- estructura física del package/repository;
- mirrors comentados ni equivalencia AST con productivo;
- comentarios/no comentarios de source;
- nombres privados;
- una implementación interna concreta cuando el comportamiento observable ya está cubierto;
- CSS visual, spacing, branding, geometría, responsive o estructura visual accidental.

La regla previa que permitía boundary tests de imports/dependencies queda **SUPERSEDED**.

## Packaging

El build es el gate natural de packaging.

No duplicar ese gate con tests que inspeccionan archivos/versiones del wheel. Si un recurso empaquetado es requisito funcional, probar que el consumidor real puede cargarlo.

## Dependencias de test

Una dependencia que el test importa directamente debe declararse como dependencia de desarrollo cuando corresponda; no convertirla en product dependency sólo para satisfacer tests.

No ocultar dependencias productivas mediante configuración pytest.

Si tests necesitan source paths de packages hermanos durante desarrollo, preferir `conftest.py` para el wiring de test antes que una lista monolítica de `pythonpath` en pytest. La dependencia productiva real sigue perteneciendo a `pyproject.toml`.

## UI

Validar manualmente:

- responsive;
- overflow;
- spacing;
- branding;
- header;
- modal shell;
- densidad;
- alineación;
- apariencia.

Un finding visual sólo genera test automatizado si puede expresarse como comportamiento contractual estable.
