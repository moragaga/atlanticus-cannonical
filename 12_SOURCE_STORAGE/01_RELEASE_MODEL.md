# Source Storage — Release Model

Estado: **DECIDED DIRECTION / CONTRACT NOT FROZEN**

## Release

Una publicación efectiva crea una nueva versión lógica Atlanticus.

No se usan deltas como mecanismo primario.

Cada release es:
- completo;
- autocontenido;
- inmutable;
- auditable.

## Draft

Guardar draft:
- NO crea release Source.

## Publish

Guardar/publicar en Source:
- valida;
- verifica Source;
- genera snapshot completo;
- genera/valida manifest/hashes;
- persiste;
- verifica persistencia;
- promueve current;
- luego proyecta ese release específico.

## Restore

Tomar un release histórico como base y publicar crea uno nuevo.

No sobrescribe el release seleccionado.

## Manifest funcional

Debe representar al menos conceptualmente:
- release/version id;
- created_at;
- created_by;
- schema_version;
- content hash;
- inventory/resource hashes;
- base version;
- previous current version.

Los nombres/campos exactos no están congelados.

## Advertencia

`backend/configuration/manifest.py` auditado actualmente representa `secrets.json`.

No es el functional Source Release Manifest y no debe reutilizarse por homonimia.
