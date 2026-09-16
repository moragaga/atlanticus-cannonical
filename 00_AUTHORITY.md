# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Último checkpoint de implementación verificado para este cierre:
  `d34cda3838a67907728b382e238f0178f9f1a64e`
- Parent inmediato verificado:
  `59fcd3ecc8f3441e64fbe0fc892b4467fa56f181`
- Alcance del checkpoint de este cierre:
  `NAVIGATION-GENERIC-CONFIGURATION-CUTOVER`

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Checkpoint canonical inspeccionado antes de este reemplazo:
  `dc7cbe626148c1b82cb2219c52cd99efafddc9d4`
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

`MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER` está cerrado.

`NAVIGATION-GENERIC-CONFIGURATION-CUTOVER` está cerrado y CURRENT en `atlanticus:main`.

El siguiente chat no debe reabrir Navigation ni asumir una solución para Users.

Único foco siguiente:

```text
USERS-MANAGER-ALIGNMENT-VALIDATION
PLANNED / NEXT
```

Objetivo: validar la desalineación ya observada entre `web/compositions/users-manager` y el contrato Manager CURRENT usando obligatoriamente `atlanticus:main` y `atlanticus-cannonical:main` antes de decidir cualquier implementación.
