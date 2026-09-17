# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Último checkpoint verificado para este cierre:
  `4e008055ddc551e6c08a7d87715340c8c7cd149e`
- Parent inmediato:
  `709cf2fb9ee422094f011cfda051f08f37276992`

El checkpoint CURRENT contiene:

```text
USERS-STANDALONE-AUTHORITY-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-BOUNDARY-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CAPABILITY-EXTRACTION
IN PROGRESS
```

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Checkpoint inspeccionado para este cierre:
  `497207bbdda23a829897751f37b9653298adf514`

`atlanticus-cannonical:main` es autoridad documental vigente, subordinada a
`atlanticus:main` cuando la implementación publicada demuestra un estado posterior.

## Referencias históricas

`moragaga/atlanticus-decisions` es **HISTORICAL**.

Puede aportar rationale y evidencia histórica. No puede reemplazar
`atlanticus:main` ni `atlanticus-cannonical:main`.

## Jerarquía

1. `atlanticus:main`: realidad implementada.
2. `atlanticus-cannonical:main`: contratos, fronteras, roadmap y estado vigente.
3. Qualification y tests vigentes: evidencia de propiedades demostradas.
4. Decisiones explícitas del Project todavía no formalizadas en canonical: delta temporal.
5. Referencias históricas indicadas por el usuario.
6. Memoria/historial conversacional: pista, nunca autoridad suficiente.

## Clasificación obligatoria

Distinguir hechos y decisiones con:

```text
VERIFIED
INFERRED
ASSUMED
PROPOSED
UNVERIFIED
```

Y estado con:

```text
CURRENT
IN PROGRESS
PLANNED
SUPERSEDED
BLOCKED
CLOSED
```

Si implementación y canonical se contradicen, exponer el conflicto y actualizar
canonical; nunca retroceder implementación CURRENT para satisfacer documentación
obsoleta.

## Git

Git es **READ ONLY** por defecto.

No crear commits, push, ramas, PR, issues ni mutaciones remotas sin autorización
explícita.

## Continuidad congelada

No reabrir sin conflicto demostrado:

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

KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-STANDALONE-AUTHORITY-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-BOUNDARY-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## Siguiente foco único

```text
USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER
PLANNED / NEXT
```

Forma parte de:

```text
PROFILES-CAPABILITY-EXTRACTION
IN PROGRESS
```

No mezclar Profiles UI, Navigation alignment, Python metadata, E2E ni cleanup
transversal de tests dentro de ese incremento.
