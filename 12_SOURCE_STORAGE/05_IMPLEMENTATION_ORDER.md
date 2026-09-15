# Source Storage — Implementation Order

Estado: **CURRENT PLAN**

## Checkpoint

```text
SOURCE-1A.1                         Core + Local                  CLOSED / VERIFIED
SOURCE-1A.2                         Blob                          CLOSED / VERIFIED
Projection                          Exact-release Core            CLOSED / VERIFIED
USERS-CANONICAL-PROJECTION-2        Users/Cosmos provider         CLOSED / VERIFIED
MANAGER-GENERIC-SOURCE-PROJECTION   Generic Manager handoff       CLOSED / VERIFIED
```

## Orden cerrado relevante

1. Source Core.
2. Local provider.
3. Blob provider.
4. Projection exact-release Core.
5. Domain providers cerrados en sus propios hitos.
6. Manager generic Source/Projection handoff.

## Manager generic handoff

CURRENT:

```text
validate          generic
read Source       generic
publish Source    generic
status            projection/core
project           ProjectionTarget
history           source/core
workspace         SourceSnapshot BASE
legacy route      NONE
exact route       NONE
```

No existe dual contract.

## Próximas fronteras

Cada consumer en incremento independiente:

```text
Step N+1  Navigation Manager consumer
Step N+2  Tools Manager consumer
Step N+3  KPI Configuration Manager consumer
Step N+4  KPI Definition Manager consumer
Step N+5  Other real consumers discovered in atlanticus:main
Step N+6  Global consumer qualification
```

## Regla por consumer

Entregar:

```text
DELETE
KEEP
MODIFY/REPLACE
GATES AFTER DELETE
```

y demostrar:

- implementación real localizada;
- contrato genérico directo;
- cero adapters/shims/aliases;
- tests scoped GREEN.

## Legacy retirement

No borrar contratos de otros dominios por inferencia.

Retirar únicamente cuando el consumer correspondiente esté localizado y cerrado.

## Regla de chat/checkpoint

Cuando un step queda CLOSED / VERIFIED:

1. actualizar canonical;
2. validar;
3. cerrar el foco;
4. abrir chat nuevo para el siguiente consumer.

No mezclar varios consumers en el mismo chat.
