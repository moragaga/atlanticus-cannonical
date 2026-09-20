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

## CLOSED — ADA Access Manager UI

```text
ACCESS-UNRESTRICTED-PROFILES-CONTRACT
CLOSED / VERIFIED / CURRENT

ACCESS-MANAGER-UI-REVIEW
CLOSED / VERIFIED MANUAL / CURRENT
```

## CLOSED — Profiles Manager UI

```text
PROFILES-MANAGER-UI-REVIEW
CLOSED / VERIFIED MANUAL / CURRENT
```

No reabrir sin conflicto demostrado:

```text
profile domain contract
profile key/color durable schema
Manager ownership of Profiles presentation
legacy modal compatibility
legacy pagination presentation
```

El cierre visual CURRENT mantiene la presentación dentro de Profiles y la metadata de entorno en
la composition.

## OPEN — Manager UI consistency

```text
MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS
```

Slices cerrados:

```text
Navigation
Accesos
Perfiles
```

Siguiente página acordada:

```text
Users
```

`Herramienta` continúa:

```text
OPEN / DEFERRED
```

Después de Users, el usuario desea cerrar el alcance actual de Manager por ahora. El cierre no
debe declarar `Herramienta` ni otros frentes diferidos como VERIFIED.

## OPEN — Users Manager UI

```text
USERS-MANAGER-UI-REVIEW
PLANNED / NEXT
```

Congelado antes de comenzar:

```text
Users = ManagerEntry
UsersAdministrationService = domain/application administration lifecycle
no synthetic Source/Projection
managed users consume ProfileCatalog
local is not a managed assignment
```

## OPEN — Responsive/media queries

```text
MANAGER-RESPONSIVE-MEDIA-QUERY-AUDIT
PLANNED / PHASE 2
```

No mezclarlo con Users salvo defecto transversal demostrado.

## OPEN — Test contract cleanup

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / PHASE 3
```

En la etapa final del alcance actual:

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

No resolver durante Users UI review.

## OPEN — Final automated qualification

Para el checkpoint final de Perfiles permanece:

```text
post-df5b targeted pytest
UNVERIFIED

post-df5b targeted Ruff
UNVERIFIED

remote CI
UNVERIFIED

full monorepo pytest
UNVERIFIED

full workspace Ruff
UNVERIFIED
```

La aceptación manual de Perfiles no sustituye esa evidencia.

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
