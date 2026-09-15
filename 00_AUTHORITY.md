# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Último checkpoint de implementación verificado para este cierre:
  `59fcd3ecc8f3441e64fbe0fc892b4467fa56f181`
- Parent inmediato verificado:
  `1302fefdf046b1cef7beed594e832f9a7a181a06`
- Alcance del checkpoint de este cierre:
  cutover genérico de `web/capabilities/manager`

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Checkpoint canonical inspeccionado antes de este reemplazo:
  `d4681dc3d14b0233c857ca3870795368456c7bad`
- Contiene estado vigente, contratos, fronteras, roadmap y decisiones activas del Project.

`atlanticus-cannonical:main` es la autoridad documental vigente, subordinada a la realidad implementada de `atlanticus:main`.

## Referencias históricas

`moragaga/atlanticus-decisions` es **HISTORICAL**.

Puede usarse para rationale, qualification previa y rastreo histórico. No puede por sí solo contradecir o reemplazar `atlanticus:main` ni `atlanticus-cannonical:main`.

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

## Regla de continuidad del Manager

Después del cutover genérico de Manager, cada consumidor se cierra en un chat/incremento independiente.

No usar un chat para mezclar Navigation, Tools, KPI Configuration y KPI Definition.
