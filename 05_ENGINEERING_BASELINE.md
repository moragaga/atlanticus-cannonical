# Atlanticus — Engineering Baseline

Estado: **CURRENT + MIGRATION PENDING**

## Runtime objetivo

- Python: `3.14.7`
- Container image: `python:3.14.7-slim-trixie`
- Base OS: Debian Trixie
- Dependency manager: `uv`
- No usar `pip` como gestor normal.

La rama auditada aún conserva `3.14.2` en múltiples `.python-version` y `pyproject.toml`.

## Web

Según necesidad:

- Python
- Dash
- Flask
- Gunicorn
- ECMAScript

## Testing

Proteger:

- comportamiento observable;
- contratos;
- invariantes;
- regresiones reales;
- callbacks relevantes;
- persistencia/recovery;
- errores;
- integración;
- flujos críticos.

No proteger por sí mismo:

- colores, margin, padding, selectores y geometría CSS;
- existencia/no existencia de funciones;
- nombres internos de clases;
- organización interna que no sea contrato.

En Web se puede comprobar existencia/carga de assets JS/CSS si es contractualmente relevante.

Responsive, branding, spacing y composición visual se califican visualmente salvo comportamiento funcional automatizable.

## Entrega

- cambios pequeños;
- alcance acordado;
- código productivo limpio;
- espejo comentado equivalente en español;
- errores en inglés;
- tests útiles;
- preferencia por ZIP integrable;
- no README parcial.

## Web startup / infrastructure policy

- Resource provisioning ocurre en startup/bootstrap de aplicación, no por iteración de job.
- Local puede asegurar base/containers cuando el contrato lo autorice.
- Cloud valida infraestructura base preexistente y asegura recursos de aplicación permitidos.
- Nunca auto-corregir partition key/TTL incompatible.
- Resource contracts deben ser declarativos y reutilizables por composición.
- Backend puede declarar requisitos sin depender de packages Web.
- Missing optional dependency produce estado degradado/no-data; invalid required contract produce error explícito.

## Reprocessing / repair policy

Los procesos incrementales pueden exponer un modo explícito de reproceso para testing/recovery.

Ese modo:
- default false;
- omite sólo gates de `already current`;
- conserva durable authority;
- conserva ordering;
- conserva leases/fencing;
- no sobreescribe conflictos write-once;
- queda registrado en observabilidad.

No reutilizar `DEBUG` para cambiar semántica de persistencia.
