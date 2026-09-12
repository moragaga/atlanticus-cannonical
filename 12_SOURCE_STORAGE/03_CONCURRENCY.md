# Source Storage — Concurrency

Estado: **DECIDED DIRECTION**

## Problema

Entre Verify Source y Publish puede cambiar `current`.

Frontend solo no elimina esa carrera.

## Responsabilidades

### Frontend / Manager
- detecta;
- presenta BASE / SOURCE / WORKSPACE;
- permite inspección;
- asiste decisión humana.

### Backend
- aplica precondición autoritativa al promover;
- evita overwrite silencioso.

## Blob

ETag / conditional write es el mecanismo natural candidato.

El contrato no debe acoplarse conceptualmente a ETag para que Local pueda implementar la misma semántica.

## Merge

No hay merge automático inicial.

La decisión sigue siendo humana hasta que exista semántica de dominio suficiente.
