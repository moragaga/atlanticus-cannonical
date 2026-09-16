# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Último checkpoint verificado para este cierre:
  `a065f45c55a527c96ce333705465487e95f0a737`
- Parent inmediato:
  `ec9bd35455b8221180b3f15740b58e34766f6112`
- Alcance:
  `USERS-CLEAN-CUTOVER-COMPLETION`

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Checkpoint inspeccionado antes de este reemplazo:
  `4b3d11974f9bbf26b72872ab43d83f1e966a5811`
- Contiene estado vigente, contratos, fronteras, roadmap y decisiones activas.

`atlanticus-cannonical:main` es autoridad documental vigente, subordinada a `atlanticus:main`.

## Referencias históricas

`moragaga/atlanticus-decisions` es **HISTORICAL**.

Puede aportar rationale y evidencia histórica. No puede contradecir o reemplazar `atlanticus:main` ni `atlanticus-cannonical:main`.

## Jerarquía

1. `atlanticus:main`: realidad implementada.
2. `atlanticus-cannonical:main`: contratos, fronteras, roadmap y estado vigente.
3. Qualification y tests vigentes: evidencia de propiedades demostradas.
4. Decisiones explícitas del Project todavía no formalizadas en canonical: delta temporal.
5. Referencias históricas indicadas por el usuario.
6. Memoria/historial conversacional: pista, nunca autoridad suficiente.

## Conflictos

Si implementación y canonical se contradicen, no resolver silenciosamente.

Clasificar como corresponda:

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

## Continuidad

No reabrir:

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-MANAGER-GENERIC-CONTRACT-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-CLEAN-CUTOVER-COMPLETION
CLOSED / VERIFIED / CURRENT

USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL
CLOSED / VERIFIED / CURRENT
```

Único foco siguiente:

```text
TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED / NEXT
```

Primero inspeccionar Tools real y contrastarlo con Manager CURRENT. No asumir cambios antes de verificar desviaciones.
