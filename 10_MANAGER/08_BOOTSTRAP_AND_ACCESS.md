# Manager — Bootstrap and Access

Estado: **CURRENT — MANAGER AUTHORIZATION CONVERGED / DUAL-PRODUCT ADOPTION NEXT**

Implementation checkpoint:

```text
moragaga/atlanticus@a75465745e188da4765e803595b17acaa55d9306
```

Decisions inspeccionado:

```text
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

## Contratos separados

```text
BOOTSTRAP ACCESS != MANAGER ACCESS
ADA ACCESS != MANAGER ACCESS
NAVIGATION AUTHORIZATION != MANAGER AUTHORIZATION
```

La autenticación crea contexto de identidad. No concede por sí misma administración Manager.

## ManagerPrincipal CURRENT

```text
subject_id
display_name
profile_keys
access_keys
is_local
administrative_override
```

La regla genérica única es:

```text
access_key is None                      → DENY
administrative_override=True            → ALLOW
access_key in principal.access_keys     → ALLOW
otherwise                               → DENY
```

Autoridad:

```text
manager_access_granted(...)
DefaultManagerAuthorizationPolicy.can_view(...)
```

`is_local=True` por sí solo no concede administración.

Un profile key por sí solo tampoco concede administración dentro de Manager Core.

La composition del producto decide cuándo emitir `administrative_override=True`.

## ADA Generic CURRENT

ADA resuelve el principal Manager desde identidad + Users.

```text
managed root
→ administrative_override=True

trusted local identity
+ local environment
→ administrative_override=True
→ profile_keys=('local',)
→ access_keys=()
→ is_local=True

basic / guest / custom
→ administrative_override=False

bootstrap root without managed user
→ administrative_override=False
```

Usuario promovido deshabilitado se rechaza.

ADA Access no participa en esta decisión.

## Command Center CURRENT

El host temporal local quedó convergido en:

```text
profile_keys=('local',)
access_keys=()
administrative_override=True
is_local=True
```

Tool Catalog usa `manager_access_granted(...)`.

Alarm Configuration usa `DefaultManagerAuthorizationPolicy.can_view(...)`.

Qualification reportada para el incremento integrado en `fd26a731...`, preservado por a7546574:

```text
Tool Catalog Manager                      9 passed
Command Center Configuration Manager     28 passed
Ruff ambos scopes                        PASS
```

## Manager access key vs ADA Access

Un `ManagerModule.access_key` / `ManagerEntry.access_key` como:

```text
users.manage
profiles.manage
navigation.manage
tools.manage
alarms.manage
```

es una capacidad administrativa del Manager.

`ManagerPrincipal.access_keys` contiene únicamente permisos administrativos Manager ya resueltos.

ADA Access posee otro contrato:

```text
profile_key → operational access_keys
```

No proyectar ADA Access sobre `ManagerPrincipal.access_keys`.

No usar visibilidad de Navigation como sustituto de Manager authorization.

## Autoridad de versión

Package owner:

```text
atlanticus-web-manager
web/capabilities/manager
CURRENT version at a7546574: 0.3.18
```

El siguiente frente debe eliminar divergencias de consumo y asegurar que ADA, Command Center, Starters y tooling resuelvan una sola autoridad de versión.

No fijar una segunda versión local.

Si la convergencia requiere un bump, se realiza en el owner y después se propaga.

## OPEN del próximo frente

La policy genérica está CLOSED.

Lo que permanece OPEN no es rediseñar autorización, sino **adoptar de forma consistente la misma composition/versión**.

Principal finding:

```text
atlanticus-web-composition-navigation-manager
BLOCKED
```

porque aún usa una API de autorización anterior y no es el camino consumido por ADA.

El siguiente frente debe converger esa composition antes de integrarla en ADA y Command Center.

## Fuera de alcance

```text
production Entra physical qualification
durable login/recovery E2E
multiworker qualification
Python 3.14.7 / Trixie migration
```

La migración Python/Trixie está **BLOCKED / DEFERRED** hasta autorización explícita del usuario.
