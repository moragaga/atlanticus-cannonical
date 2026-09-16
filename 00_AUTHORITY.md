# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Último checkpoint verificado para este cierre:
  `ef3f0a44c5dcc14f8fcafe5bb36bb97865381924`
- Parent inmediato:
  `4c7f8aa8b541e8b8f8abc7b49fe22526a4952bfe`
- Alcance cerrado por ese checkpoint:
  `KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER`

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Checkpoint inspeccionado antes de este reemplazo:
  `430a90529c99e69d16978f60d91aa86f819b851e`
- Contiene estado vigente, contratos, fronteras, roadmap y decisiones activas.

`atlanticus-cannonical:main` es autoridad documental vigente, subordinada a `atlanticus:main` cuando la implementación publicada demuestra un estado posterior.

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

Si implementación y canonical se contradicen, exponer el conflicto y actualizar canonical; nunca retroceder implementación CURRENT para satisfacer documentación obsoleta.

## Git

Git es **READ ONLY** por defecto.

No crear commits, push, ramas, PR, issues ni mutaciones remotas sin autorización explícita.

## Continuidad congelada

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

KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Tools, KPI Configuration y KPI Definition conservan ownership ADA bajo `scopes/ada` y consumen directamente infraestructura Source/Projection genérica donde corresponde.

No reintroducir contratos legacy en esos dominios para sostener consumidores antiguos.

## Siguiente foco único

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
PLANNED / NEXT
```

El consumer `scopes/ada/web/application/ada-configuration-manager` permanece desalineado con contratos ya migrados. Debe cortarse una sola vez al contrato genérico CURRENT; no crear adapters, aliases, shims ni doble routing para conservar la arquitectura anterior.
