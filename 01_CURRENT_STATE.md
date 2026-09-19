# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

## Autoridad

Implementación publicada CURRENT:

```text
moragaga/atlanticus@9f12c41a23d69784c7c5b775a4093a94ac654d55
```

Parent inmediato:

```text
3eb46dac80f23d438774e3afa39999dc96f592d7
```

Tree:

```text
dd002b632b494065428af9dd10f1e58b7e6638d1
```

Canonical inspeccionado para este cierre:

```text
moragaga/atlanticus-cannonical@b11b6ad4fd8d32ba89d029e4d200fc42d6933091
```

Git permanece SOLO LECTURA para el asistente.

## Estado resumido

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER               CLOSED / VERIFIED / CURRENT
PROFILES-CONFIGURATION-BOUNDARY-CUTOVER          CLOSED / VERIFIED / CURRENT
PROFILES-CAPABILITY-EXTRACTION                   CLOSED / VERIFIED / CURRENT
PROFILES-INDEPENDENT-SOURCE-LIFECYCLE            CLOSED / VERIFIED / CURRENT
USERS-PERSISTED-DATA-CUTOVER                     CLOSED / VERIFIED / CURRENT
ADA-ACCESS-PROFILES-CONFIGURATION                CLOSED / VERIFIED / CURRENT
NONPROMOTED-ACCESS-SEMANTICS-CORRECTION          CLOSED / VERIFIED / CURRENT
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT         CLOSED / VERIFIED / CURRENT
MANAGER-AUTHORIZATION-SEMANTICS-ALIGNMENT        CLOSED / VERIFIED / CURRENT
MANAGER-ACTIVE-WORKFLOW-CALLBACK-CARDINALITY     CLOSED / VERIFIED / CURRENT
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT BLOCKED / VERIFIED CONFLICT
CONFIGURATION-UI-COMPOSITION-RECOVERY             PLANNED / NEXT
USERS-ADMINISTRATION-SURFACE-CUTOVER             PLANNED / SEPARATE
ADA-ACCESS-RUNTIME-COMPOSITION                   OPEN / SEPARATE
WEB-TEST-CONTRACT-CLEANUP                        PLANNED / OPEN
PYTHON-METADATA-ALIGNMENT                        PLANNED / OPEN
```

## VERIFIED

### Published checkpoint

`main` está publicado exactamente en:

```text
9f12c41a23d69784c7c5b775a4093a94ac654d55
```

con parent:

```text
3eb46dac80f23d438774e3afa39999dc96f592d7
```

### Manager authorization semantics

`ManagerModuleAccess` fue removido del contrato CURRENT.

`ManagerModule` expone:

```text
access_key: str | None
```

`ManagerAuthorizationPolicy` expone:

```text
can_view(principal, module) -> bool
```

`DefaultManagerAuthorizationPolicy` concede acceso sólo si el `access_key` del módulo
está presente en `principal.access_keys`.

No conceden acceso por sí mismos:

```text
principal.is_local
'administrator' in principal.profile_keys
```

El coordinator usa una única verificación de acceso de módulo antes de:

```text
status
projection target
validate draft
source snapshot/current source
publish
project
history
```

No existen permisos Manager separados por operación para validate/publish/project.

### ADA Configuration Manager authorization

La composition CURRENT declara access keys funcionales:

```text
navigation.manage
tools.manage
kpis.manage
```

Navigation y Tools usan sus keys respectivas.
KPI Configuration y KPI Definition comparten `kpis.manage`.

Los callbacks/domain contexts específicos usan `_has_access(principal, access_key)` y no
bypass de `is_local` o profile `administrator`.

El runtime local conserva `is_local=True` como contexto y recibe access keys explícitos.

### Manager active workflow callback

`refresh_active_workflow` usa `PreventUpdate` cuando durante transición de ruta no existe
un módulo visible resoluble.

Esto reemplaza el retorno de listas vacías que provocaba cardinalidad inválida con Outputs
pattern `ALL`.

Existe test de regresión específico en `test_dash_registration.py`.

### Qualification observada durante el hito

Antes del último delta de callback se observó:

```text
legacy Manager authorization scan
0 matches en el scope buscado

Manager + navigation-manager focused pytest
68 PASS

web full pytest
PASS / 100%
7 skipped

ADA Configuration Manager pytest
26 PASS

focused Ruff / format
PASS

ADA Configuration Manager uv lock --check
PASS después de alinear navigation-configuration 0.1.9
```

Después del delta final del callback se observó:

```text
callbacks productive/commented AST-equivalent
PASS

targeted callback regression tests
PASS

git diff --check
PASS dentro del script de reparación

manual smoke ADA Configuration Manager
/manager carga
navegación entre superficies responde 200/204
sin InvalidCallbackReturnValue
sin HTTP 500 observado
```

No declarar que el full web pytest ni el full ADA pytest fueron rerun después del último
delta del callback: no se observó esa ejecución final completa.

### Dependency lock alignment

ADA Configuration Manager fue alineado de:

```text
atlanticus-web-navigation-configuration[web]==0.1.8
```

a:

```text
atlanticus-web-navigation-configuration[web]==0.1.9
```

`uv.lock` quedó actualizado y `uv lock --check` pasó.

### UI administrativa CURRENT

ADA Configuration Manager CURRENT compone:

```text
navigation
tools
kpis
kpi-definitions
```

El árbol CURRENT verifica que:

```text
web/capabilities/profiles/configuration
```

posee modelos y Source lifecycle, pero no superficie/editor Dash.

```text
web/capabilities/users/core
```

posee `UsersAdministrationService`, pero no superficie administrativa web.

```text
scopes/ada/web/access/configuration
```

posee modelo/configuración + Source lifecycle, pero no superficie/editor Dash.

Por tanto, la ausencia CURRENT de esas tres UI está VERIFIED.

## VERIFIED CONFLICT

### navigation-manager standalone consumer

En CURRENT:

```text
ManagerAuthorizationPolicy.can_view(...)
```

es el contrato disponible.

Pero:

```text
web/compositions/navigation-manager/src/.../composition.py
```

invoca:

```text
resolved_authorization.can_access(...)
```

Ese consumer no está alineado con el contrato publicado.

Estado:

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

El smoke de ADA Configuration Manager no demuestra ese consumer porque ADA Configuration
Manager compone Navigation por su propia composition.

## INFERRED

El contrato de autorización publicado expresa una capacidad funcional por módulo, no
permisos por cada paso interno del workflow.

La ausencia de superficies UI para Profiles, Users Administration y ADA Access no exige
nuevos dominios: existe lógica/backend que la UI futura debe consumir.

La recuperación de una composición visual transversal debe preferir código/histórico
verificable antes que recreación manual.

## ASSUMED

No se asume:

- que una UI histórica concreta sea todavía correcta;
- que Profiles, Users y Access deban compartir el mismo lifecycle de Manager;
- que Users deba volver a Source/Projection;
- que ADA Access deba depender de Navigation;
- que el consumer standalone navigation-manager esté funcional hasta corregir `can_access`;
- que full CI esté verde;
- que todo Ruff workspace esté limpio;
- que metadata Python esté globalmente alineada.

## PROPOSED

Único foco siguiente:

```text
CONFIGURATION-UI-COMPOSITION-RECOVERY
PLANNED / NEXT
```

Debe comenzar por inventario y recuperación, no por diseño nuevo.

## UNVERIFIED / OPEN

```text
visualizaciones/composiciones UI históricas concretas a recuperar
UNVERIFIED HISTORICAL

exact guest fallback composition for authenticated non-promoted identities
OPEN / SEPARATE

Users Administration UI
PLANNED

Profiles Configuration UI
PLANNED

ADA Access Configuration UI
PLANNED

ADA Access runtime composition exacta
OPEN / SEPARATE

concrete Entra/Graph UsersDirectoryReader provider
UNVERIFIED

full web pytest después del último callback delta
UNVERIFIED

full ADA pytest después del último callback delta
UNVERIFIED

CI remoto de 9f12c41...
UNVERIFIED

full Ruff workspace de 9f12c41...
UNVERIFIED

Python metadata global 3.14.7
OPEN / SEPARATE
```
