# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Último checkpoint verificado para este cierre:
  `27c2e4beed125fe379881048f0df5fbe3ff6cb1a`
- Parent inmediato:
  `a065f45c55a527c96ce333705465487e95f0a737`
- Alcance:
  `TOOLS-GENERIC-SOURCE-PROJECTION-CUTOVER`

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Checkpoint inspeccionado antes de este reemplazo:
  `11bb50752b319bab40ab57f68fe1ae6299c71c41`
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

TOOLS-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Tools conserva ownership ADA bajo `scopes/ada/web/tools` y consume directamente Source/Projection genéricos.

El consumer `ada-configuration-manager` puede permanecer temporalmente desalineado. No crear compatibilidad dentro de Tools para sostenerlo.

Único foco siguiente:

```text
KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
PLANNED / NEXT
```

Inspeccionar únicamente KPI Configuration CURRENT y contrastarlo contra el patrón Tools publicado. No tocar Manager ni KPI Definition en el mismo incremento.
