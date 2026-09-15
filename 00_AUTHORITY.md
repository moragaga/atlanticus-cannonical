# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Último checkpoint de implementación verificado para este cierre:
  `384a68fe8fa42263623c95d1d132af2ca54574c8`
- Fecha del checkpoint:
  `2026-09-15T17:15:17Z`
- Parent inmediato verificado:
  `b2254450b4543d2422ca8580357b9054b515cd6e`

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Checkpoint canonical inspeccionado antes de este reemplazo:
  `b58a6c8f0f7631de6789adaee4b913b197be8806`
- Contiene estado vigente, contratos, fronteras, roadmap y decisiones activas del Project.

`atlanticus-cannonical:main` reemplaza a `atlanticus-decisions` como autoridad documental vigente.

## Referencias históricas

`moragaga/atlanticus-decisions` es **HISTORICAL**.

Puede usarse para rationale, qualification previa y rastreo histórico. No puede por sí solo contradecir o reemplazar `atlanticus-cannonical:main`.

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
