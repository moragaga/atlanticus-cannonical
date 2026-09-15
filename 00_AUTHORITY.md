# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Último checkpoint de implementación verificado para este cierre:
  `d23bff025ab899367a8da1178dde5ab50806fe47`
- Fecha del checkpoint:
  `2026-09-15T13:09:28Z`
- Parent inmediato verificado:
  `7ffebdbb0b70e41c6f0bd903cc7f27dbd3a05d98`

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Checkpoint canonical inspeccionado antes de este reemplazo:
  `25e1f1bee4548ed7a7c35d2d1ed9a252c51b5397`
- Contiene el estado vigente, contratos, fronteras, roadmap y decisiones activas del Project.

`atlanticus-cannonical:main` reemplaza a `atlanticus-decisions` como autoridad documental vigente.

## Referencias históricas

`moragaga/atlanticus-decisions` es **HISTORICAL**.

Puede utilizarse para rationale histórico, qualification previa, rastreo de decisiones anteriores y evidencia de cómo se llegó a un contrato.

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
