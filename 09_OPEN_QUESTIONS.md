# Atlanticus — Open Questions

Estado: **CANONICAL OPEN ITEMS**

Los puntos aquí no reabren contratos CLOSED.

## CLOSED — Manager generic core

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

No están OPEN:

```text
double routing exact/legacy
workflow_service lifecycle
ExactProjectionWorkflow
expected_source_revision
revision -> ProjectionTarget reconstruction
compatibility shims/adapters
```

## CLOSED — Configuration domains

```text
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
```

## CLOSED — Users / Profiles / Access / Navigation alignment sequence

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-PERSISTED-DATA-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CAPABILITY-EXTRACTION
CLOSED / VERIFIED / CURRENT

PROFILES-INDEPENDENT-SOURCE-LIFECYCLE
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROFILES-CONFIGURATION
CLOSED / VERIFIED / CURRENT

NONPROMOTED-ACCESS-SEMANTICS-CORRECTION
CLOSED / VERIFIED / CURRENT

NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
CLOSED / VERIFIED / CURRENT
```

Ya no está OPEN cómo Navigation Configuration obtiene Profiles para administración y
validación: consume `ProfileCatalog` desde Profiles core mediante composition.

Tampoco están OPEN:

```text
NavigationProfileOption
_BASE_PROFILES
NavigationProfileOptionsProvider
profile_options_provider
Navigation -> Users profile options
Navigation -> ADA Access authorization dependency
```

## OPEN — Manager authorization stale semantics

Implementación CURRENT todavía contiene en `DefaultManagerAuthorizationPolicy`:

```text
principal.is_local
OR
'administrator' in principal.profile_keys
→ full Manager access
```

ADA Configuration Manager mantiene helpers `_can_manage_navigation`, `_can_manage_tools`
y `_can_manage_kpis` con bypass equivalente.

Preguntas obligatorias del siguiente chat recomendado:

1. ¿Cuál es el contrato final de `ManagerPrincipal` para autorización?
2. ¿Debe `ManagerModuleAccess` ser la única fuente funcional de permisos por módulo?
3. ¿Cómo obtiene permisos explícitos el runtime local sin inventar `administrator`?
4. ¿Qué responsabilidad pertenece al generic Manager y cuál a la composition ADA?
5. ¿Qué tests de comportamiento deben proteger el contrato final?
6. ¿Qué referencias `administrator`/`is_local` quedan realmente legacy y cuáles son runtime concerns legítimos?

No implementar hasta revisar código CURRENT y consumers.

Estado:

```text
OPEN / PROPOSED NEXT
```

## OPEN — exact Navigation fallback para identidad no promovida

La entrada a la aplicación ya es CURRENT para identidad autenticada no promovida.

Sigue OPEN la composición exacta de `NavigationPrincipal`/perfil de fallback.

No resolver mediante:

```text
guest authority en Users
UserRecord ficticio
Navigation -> Users dependency
Navigation -> ADA Access dependency
```

Estado:

```text
OPEN / SEPARATE
```

## OPEN — Users Administration surface

```text
USERS-ADMINISTRATION-SURFACE-CUTOVER
PLANNED / SEPARATE
```

No reintroducir Users en Configuration Manager como Source/Projection.

## OPEN — concrete Entra directory discovery

Contrato disponible:

```text
UsersDirectoryReader
```

Provider concreto Graph/Entra:

```text
UNVERIFIED
```

No inventar tenant settings, Graph permissions, credential flow ni endpoints.

## OPEN — ADA Access runtime composition

ADA Access domain/configuration está cerrado.

El wiring runtime exacto permanece separado de Navigation Configuration y no fue
modificado en este hito.

Estado:

```text
OPEN / SEPARATE
```

## OPEN — Python package metadata alignment

Canonical fija:

```text
Python 3.14.7
```

Navigation Configuration CURRENT:

```text
requires-python = "==3.14.2"
```

La qualification local del último cutover usó Python 3.14.7, pero la metadata continúa
inconsistente.

```text
PYTHON-METADATA-ALIGNMENT
PLANNED / OPEN
```

## PLANNED — Web test contract cleanup

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN
```

Durante qualification del último hito quedaron findings fuera de alcance en:

```text
capabilities/navigation/configuration/tests/test_web_contract.py
capabilities/navigation/configuration/tests/test_web_source_contract.py
```

No fueron modificados oportunistamente.

## UNVERIFIED

- concrete Entra/Graph directory provider;
- full Ruff workspace después de `3eb46dac...`;
- CI remoto para `3eb46dac...`;
- Python metadata/Trixie global qualification;
- exact runtime fallback guest composition;
- exact local Manager authorization composition después del futuro cutover.

## Siguiente foco recomendado

```text
Manager authorization stale administrator/local semantics
```

Fuentes obligatorias:

```text
moragaga/atlanticus:main
moragaga/atlanticus-cannonical:main
```

`moragaga/atlanticus-decisions` es sólo HISTORICAL.

Git sólo lectura para el asistente.
