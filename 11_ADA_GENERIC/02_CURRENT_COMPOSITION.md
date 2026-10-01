# ADA Generic — Current Composition

Estado: **CURRENT / MANAGER AUTHORIZATION CONVERGED / MANAGER COMPOSITION CONVERGENCE NEXT**

Implementation:

```text
moragaga/atlanticus@a75465745e188da4765e803595b17acaa55d9306
```

Decisions:

```text
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
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

VERIFIED:

```text
atlanticus-web-composition-users-manager
→ consumed

atlanticus-web-composition-profiles-manager
→ consumed

atlanticus-web-composition-navigation-manager
→ NOT consumed
```

ADA Configuration Manager conserva hoy wiring/workflows propios de Navigation.

No declarar Navigation reusable convergido hasta que esa diferencia sea resuelta y ADA consuma el contrato común.

## Navigation binding operacional

ADA Generic mantiene separado:

```text
ManagerPrincipal
    ↓ composition binding
NavigationPrincipal
```

Esto es una traducción legítima entre capabilities con authorization contracts distintos.

No fusionar Manager authorization con Navigation authorization.

No introducir ADA Access para cerrar esa frontera.

## Generated Tool CURRENT

El Starter/Tool ADA generado conserva:

```text
application.pages
application.modules
```

sobre la composición reusable ADA Generic.

El project tooling reusable fue extraído a `ada-project-tooling`.

La distribución trazable CURRENT de `a7546574` conserva el estado documentado en `17_DISTRIBUTION_AND_TOOLING/03_ARTIFACT_DISTRIBUTION.md`.

## Python CURRENT y migración

Los paquetes/distribución relevantes continúan sobre Python `3.14.2`.

La migración a Python 3.14.7 / `python:3.14.7-slim-trixie` queda:

```text
BLOCKED / DEFERRED UNTIL EXPLICIT USER AUTHORIZATION
```

No convertirla en gate ni trabajo incidental de este frente.

## Decisión de secuencia refinada

La secuencia previa:

```text
qualify generated ADA runtime
→ otros productos después
```

queda refinada para el Manager:

```text
1. converge reusable Manager compositions/version
2. integrate same authority into ADA Generic
3. integrate same authority into ADA Command Center
4. align tooling/distribution of both products
5. qualify both integrations
```

El mismo chat debe completar 1–5 antes de abrir otro frente.

Esto no autoriza cambios en Tool→Tool, KPI, Alarm Engine, Live ni Analytics.

## Próximo foco

```text
MANAGER-COMPOSITION-CONVERGENCE-AND-DUAL-PRODUCT-INTEGRATION
PLANNED / NEXT
```

No crear adapters legacy ni una segunda composition paralela para conservar el wiring reemplazado.
