# Source Storage — SourceStore Contract

Estado: **TO FREEZE BEFORE IMPLEMENTATION**

## Objetivo

Local y Azure Blob deben exponer la misma semántica al resto del sistema.

La infraestructura cambia; el modelo funcional no.

## Operaciones conceptuales mínimas

- create release;
- get release;
- list history;
- get current;
- promote release;
- verify integrity.

Los nombres definitivos de APIs aún no están congelados.

## Providers

### Local
Primer provider recomendado para cerrar semántica y tests sin costo remoto.

### Azure Blob Storage
Segundo provider del mismo contrato.

No crear comportamiento funcional distinto por provider.

## Storage connectivity disponible

Atlanticus ya dispone de una capability Storage con:
- connection string;
- SAS;
- tipado explícito;
- protección de secretos en repr;
- URL validation.

Eso es conectividad, no `SourceStore`.

No mezclar ambas responsabilidades.
