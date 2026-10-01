# ADA Generic — Current Composition

Estado: **CURRENT / MANAGER + USERS + PROFILES + NAVIGATION COMPOSITIONS CONVERGED**

Implementation:

```text
moragaga/atlanticus@36361dd570f86e8350ea4a6ee0e09bab351ba171
```

Decisions de referencia inspeccionadas durante la convergencia:

```text
moragaga/atlanticus-decisions:main
manager_decisions/ATLANTICUS_MANAGER_GLOBAL_RULES_2026-09-02.md
```

## Composition CURRENT

ADA Generic es el composition root/host integrado de ADA.

No ejecuta ADA Configuration Manager como proceso remoto.

Cuando dispone de Manager dependencies/stores:

```text
ADA Generic
    ↓ imports
build_configuration_manager_surface(...)
    ↓
ManagerSurface
    ↓
integrate_manager_surface(...)
    ↓
same WebApplicationDefinition / same Flask-Dash runtime
```

El host standalone `ada-configuration-manager` existe para desarrollo/qualification y comparte la composition; no es un segundo servicio requerido por ADA Generic.

## Manager CURRENT

Autoridad:

```text
atlanticus-web-manager==0.3.19
```

Autorización:

```text
managed root                         → administrative_override
trusted local + local environment    → administrative_override
basic / guest / custom               → no Manager administration
bootstrap root                       → no implicit Manager administration
```

`ManagerPrincipal.access_keys` permanece vacío para root/local globales.

ADA Access no es autoridad Manager.

## Compositions consumidas actualmente por ADA

VERIFIED / CURRENT:

```text
atlanticus-web-composition-users-manager==0.1.1
→ consumed

atlanticus-web-composition-profiles-manager==0.2.0
→ consumed

atlanticus-web-composition-navigation-manager==0.3.0
→ consumed
```

ADA Configuration Manager ya no conserva workflows/service IDs bespoke de Navigation.

ADA injecta explícitamente:

```text
SourceKey('navigation')
group_key='configuration'
title='Navegación'
route='/navigation'
order=20
access_key='navigation.manage'
ProfileCatalog → NavigationProfileOption
```

No existe alias entre `navigation` y el default reusable `navigation-configuration`.

## Navigation binding operacional

ADA Generic mantiene separado:

```text
ManagerPrincipal
    ↓ composition binding
NavigationPrincipal
```

También conserva explícitamente el store de proyección necesario para consumo operacional:

```text
ConfigurationManagerDependencies.navigation_projection_store
    ↓
create_projected_navigation_definition_provider(...)
    ↓
operational Navigation
```

Este store es una dependencia operacional real. No es un shim ni restaura el wiring administrativo reemplazado.

No fusionar Manager authorization con Navigation authorization.

No introducir ADA Access para cerrar esa frontera.

## Versiones CURRENT del cierre

```text
ada-configuration-manager==0.7.1
ada-generic-application==0.2.20
atlanticus-web-manager==0.3.19
atlanticus-web-composition-navigation-manager==0.3.0
```

Qualification local reportada durante el cierre:

```text
ADA Configuration Manager   65 PASS
ADA Generic                288 PASS
Ruff                         PASS
git diff --check             PASS
legacy Navigation rg         EMPTY
```

Esta evidencia no equivale a Docker/Azure ni al Golden Path durable.

## Generated Tool CURRENT

El Starter/Tool ADA generado conserva:

```text
application.pages
application.modules
```

sobre la composición reusable ADA Generic.

El project tooling reusable fue extraído a `ada-project-tooling`.

No se reabre tooling como siguiente foco dual todavía porque Command Center aún no tiene application generic equivalente.

## Python CURRENT y migración

Los paquetes Web relevantes de este corte continúan declarando Python `3.14.2`.

La migración a Python 3.14.7 / `python:3.14.7-slim-trixie` queda:

```text
BLOCKED / DEFERRED UNTIL EXPLICIT USER AUTHORIZATION
```

No convertirla en gate ni trabajo incidental.

## Secuencia refinada

La secuencia anterior:

```text
Manager convergence
→ ADA
→ Command Center
→ tooling/distribution dual
```

queda refinada porque Command Center todavía no posee composition root final:

```text
1. Manager convergence reusable                         CLOSED
2. ADA Generic integration                              CLOSED
3. Command Center existing Manager components alignment CLOSED locally
4. design/implement Command Center Generic Application  NEXT
5. only then align dual-product tooling/distribution    PLANNED
6. qualification/distribution gates                     PLANNED
```

## Próximo foco

ADA Generic no requiere cambios en el próximo chat salvo finding real.

El próximo foco pertenece a Command Center:

```text
ADA-COMMAND-CENTER-GENERIC-APPLICATION-COMPOSITION
PLANNED / NEXT
```
