# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Último checkpoint de implementación verificado para este cierre:
  `d63886d8d42688e3d03d680f0a3d7b92cd3863ed`
- Fecha del checkpoint: `2026-09-13T17:57:34Z`

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Contiene el estado vigente, contratos, fronteras, roadmap y decisiones activas del Project.

`atlanticus-cannonical:main` reemplaza a `atlanticus-decisions` como autoridad documental vigente.

## Referencias históricas

`moragaga/atlanticus-decisions` es **HISTORICAL**.

Puede utilizarse para:
- rationale histórico;
- qualification previa;
- rastreo de decisiones anteriores;
- evidencia de cómo se llegó a un contrato.

No puede utilizarse por sí solo para contradecir o reemplazar el estado vigente de `atlanticus-cannonical:main`.

## Jerarquía

1. `atlanticus:main`: realidad implementada actual.
2. `atlanticus-cannonical:main`: contratos, fronteras, roadmap y estado vigente.
3. Qualification y tests vigentes: evidencia de propiedades demostradas.
4. Decisiones explícitas del Project todavía no formalizadas en canonical: delta temporal.
5. Repositorios o documentos históricos indicados expresamente por el usuario: referencia.
6. Memoria/historial conversacional: pista de búsqueda, nunca autoridad suficiente por sí sola.

## Conflictos

Si `atlanticus:main` y canonical se contradicen, no resolver silenciosamente.

Clasificar explícitamente como corresponda:
- `VERIFIED`
- `INFERRED`
- `ASSUMED`
- `PROPOSED`
- `UNVERIFIED`
- `CURRENT`
- `IN PROGRESS`
- `PLANNED`
- `SUPERSEDED`
- `BLOCKED`
- `CONFLICT`
- `HISTORICAL`

## Git

Git es **READ ONLY** por defecto.

No crear commits, push, ramas, PR, issues ni mutaciones remotas sin autorización explícita.

## Referencias externas

Otros repositorios, proyectos o implementaciones sólo son referencias cuando el usuario lo indique.

No transfieren automáticamente autoridad, contratos, nombres, dependencias ni arquitectura a Atlanticus.
