# Manager — Bootstrap and Access

Estado: **CURRENT DIRECTION / REFINED AFTER USERS AND PROFILES CUTOVERS**

## Alcance

Este documento conserva la frontera entre Bootstrap Access y Manager Access.

El cierre actual no modifica la autorización interna de Manager; únicamente corrige
las referencias antiguas que acoplaban Navigation a Users para obtener perfiles.

## Bootstrap Access

Bootstrap puede existir antes de que otras capabilities de aplicación estén listas.

```text
BOOTSTRAP ACCESS
        ≠
MANAGER ACCESS
```

Producción utiliza identidad autenticada mediante el provider configurado.

El estado CURRENT de Identity/Users distingue:

```text
invalid identity
→ rejected

promoted disabled user
→ USER_DISABLED / 403

valid authenticated identity without promoted UserRecord
→ READY
```

La promoción de Users no es el gate de entrada a la aplicación.

## Manager Access

Manager utiliza su propio contrato de autorización.

Los detalles actuales de `profile_keys`, `access_keys`, `is_local` y cualquier semántica
stale relacionada con `administrator` pertenecen a un frente separado.

Este documento no redefine esa política ni crea una nueva.

## Superficie previa

Una superficie bootstrap/operacional puede existir antes de que Manager tenga todas sus
dependencias listas para permitir diagnóstico y recuperación controlada.

La ruta, UI y composición exactas siguen fuera del alcance de este cierre.

## Local

Provider local y autoridad local son runtime concerns.

No existe mapping contractual:

```text
local -> administrator
```

La semántica exacta de Manager para `is_local` no se modifica en este hito.

## Navigation / Users / Profiles

Manager debe poder convivir con capabilities instaladas de forma independiente.

```text
Users only
Navigation only
Users + Navigation
Profiles + Navigation
```

Navigation no debe requerir Users ni ADA Access.

Cuando Navigation necesita catálogo de perfiles para validación o administración,
la integración correcta es con la capability generic Profiles:

```text
Profiles
    ↓ optional integration
Navigation
```

No usar:

```text
Users -> Navigation profile options
ADA Access -> Navigation authorization
```

Navigation conserva sus propias `allowed_profiles` como referencias por key.

## Estado

```text
BOOTSTRAP / ACCESS SEPARATION
CURRENT DIRECTION

USERS NONPROMOTED ENTRY SEMANTICS
CLOSED / VERIFIED / CURRENT

NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
PLANNED

MANAGER AUTHORIZATION CLEANUP
OPEN / OUT OF CURRENT FOCUS
```
