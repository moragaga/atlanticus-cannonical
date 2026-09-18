# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Checkpoint CURRENT verificado para este cierre:
  `6dd09a6f24370bbad8ae358b6d5d7c6ea9aeba4a`
- Parent inmediato:
  `4e008055ddc551e6c08a7d87715340c8c7cd149e`
- Tree:
  `618619a6cb0fb416d51e7b095e1ed0a1d743a4c9`

El checkpoint CURRENT contiene:

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-BOUNDARY-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CAPABILITY-EXTRACTION
IN PROGRESS
```

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Checkpoint inspeccionado antes de este reemplazo:
  `61da5829c6a1f8ec936d46e5a7ec02965b5e4743`

`atlanticus-cannonical:main` es autoridad documental vigente, subordinada a
`atlanticus:main` cuando la implementación publicada demuestra un estado posterior.

## Referencias históricas

`moragaga/atlanticus-decisions` es **HISTORICAL**.

Puede aportar rationale y evidencia histórica. No puede reemplazar
`atlanticus:main` ni `atlanticus-cannonical:main`.

No se usa una decisión histórica para reintroducir Users como Source de configuración,
recrear contratos Users/Profiles combinados ni conservar legacy eliminado.

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

TOOLS-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-BOUNDARY-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Los siguientes hitos anteriores de Users quedan históricos y no definen el contrato
CURRENT:

```text
USERS-MANAGER-GENERIC-CONTRACT-CUTOVER
CLOSED / SUPERSEDED

USERS-CLEAN-CUTOVER-COMPLETION
CLOSED / SUPERSEDED

USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL
CLOSED / SUPERSEDED

USERS-STANDALONE-AUTHORITY-CUTOVER
CLOSED / SUPERSEDED BY USERS-GLOBAL-REGISTRY-ROOT-CUTOVER

USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER
SUPERSEDED / NOT EXECUTED AS FINAL TARGET

USERS-PROFILES-COMPOSITION-CUTOVER
SUPERSEDED AS PREVIOUS MODEL
```

## Siguiente foco único

```text
USERS-PERSISTED-DATA-CUTOVER
PLANNED / NEXT
```

Su primera etapa debe ser inventario y diseño contra datos/topología reales.
No inventar migración, Entra provider, containers, credenciales, perfiles ni datos
que no estén demostrados por las fuentes autoritativas.

No mezclar Users Administration UI, Profiles lifecycle, Access, Navigation, Python
metadata ni cleanup transversal de tests dentro de ese incremento.
