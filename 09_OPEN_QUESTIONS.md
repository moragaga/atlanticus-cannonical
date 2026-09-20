# Atlanticus — Open Questions

Estado: **CANONICAL OPEN ITEMS**

Los puntos aquí no reabren contracts CLOSED.

## CLOSED — Navigation standalone configuration

```text
NAVIGATION-STANDALONE-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT

NAVIGATION-PUBLIC-ACCESS-CONTRACT
CLOSED / VERIFIED / CURRENT

NAVIGATION-PROFILE-OPTIONS-DECOUPLING
CLOSED / VERIFIED / CURRENT

NAVIGATION-CONFIGURATION-UI-PASS
CLOSED / VERIFIED MANUAL / CURRENT
```

No reabrir:

```text
Navigation Configuration -> Profiles core hard dependency
separate profiles context card
guest auto-selection
empty profile list as implicit deny
```

## OPEN — Manager UI consistency

```text
MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS
```

Navigation está cerrado dentro de este review.

Siguiente página:

```text
Herramienta
```

La fase actual sólo debe resolver presentación/consistencia de páginas Manager.

## OPEN — Responsive/media queries

```text
MANAGER-RESPONSIVE-MEDIA-QUERY-AUDIT
PLANNED / PHASE 2
```

Existe código CURRENT en shared Manager CSS con bottom padding `0` en
`.atlanticus-manager__module-page`, incluida la regla `@media (max-width: 48rem)`.

La intención visual global de ese cambio no fue calificada en este cierre.

No asumirlo correcto ni incorrecto sin revisar las páginas y breakpoints.

## OPEN — Test contract cleanup

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / PHASE 3
```

En la etapa final del UI review:

- ejecutar targeted tests y Ruff;
- conservar behavior/contracts/invariants;
- eliminar tests cuyo único propósito sea CSS, visual structure, selectors, clases internas,
  funciones internas o implementación accidental;
- no modificar la UI para satisfacer un test visual inválido.

## OPEN — Navigation Manager authorization consumer

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

CURRENT:

```text
ManagerAuthorizationPolicy.can_view(...)
```

Consumer actual:

```text
web/compositions/navigation-manager
→ authorization.can_access(...)
```

No añadir compatibility alias.

No resolver durante Manager UI review.

## OPEN — Final automated qualification

La última evidencia automatizada observada antes de los ajustes visuales finales fue:

```text
Navigation core                           22 passed
Navigation Configuration                 42 passed
Navigation Manager                       10 passed
ADA Configuration Manager focused         5 passed
```

Post-checkpoint CURRENT:

```text
post-29bb targeted pytest
UNVERIFIED

post-29bb targeted Ruff
UNVERIFIED

remote CI
UNVERIFIED
```

Esto se cierra en PHASE 3 del UI review, no antes.

## OPEN — Real persistence qualification

```text
MANAGER-REAL-PERSISTENCE-QUALIFICATION
PLANNED / AFTER UI REVIEW
```

No confundir render/UI correctness con persistencia real.

## Separados

```text
ADA Access runtime composition
PLANNED / SEPARATE

Navigation disabled-route surface
PLANNED / SEPARATE

concrete Entra/Graph provider
UNVERIFIED

Python metadata alignment
PLANNED / SEPARATE

global CI/workspace cleanup
PLANNED / SEPARATE
```
