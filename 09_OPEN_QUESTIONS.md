# Atlanticus — Open Questions

Estado: **CANONICAL OPEN ITEMS**

Los puntos aquí no reabren contratos CLOSED.

## CLOSED — Manager authorization semantics

```text
MANAGER-AUTHORIZATION-SEMANTICS-ALIGNMENT
CLOSED / VERIFIED / CURRENT
```

Ya no están OPEN:

```text
si is_local debe conceder acceso Manager
si administrator profile debe conceder acceso Manager
si ManagerModuleAccess debe separar view/validate/publish/project
cómo obtiene permisos el runtime local
```

CURRENT:

```text
ManagerModule.access_key
ManagerAuthorizationPolicy.can_view
explicit principal.access_keys
```

`is_local` no concede autoridad y el runtime local recibe capabilities explícitas.

## CLOSED — Manager callback cardinality

```text
MANAGER-ACTIVE-WORKFLOW-CALLBACK-CARDINALITY
CLOSED / VERIFIED / CURRENT
```

La transición sin módulo resoluble usa `PreventUpdate`.

## OPEN — navigation-manager authorization consumer mismatch

Implementación CURRENT contiene:

```text
ManagerAuthorizationPolicy.can_view(...)
```

pero `web/compositions/navigation-manager` llama:

```text
resolved_authorization.can_access(...)
```

Estado:

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

No introducir `can_access` como alias de compatibilidad.
Cuando ese consumer forme parte del siguiente foco, debe alinearse directamente al contrato CURRENT.

## OPEN — configuration UI composition recovery

Ausencia CURRENT verificada:

```text
Profiles Configuration UI
Users Administration UI
ADA Access Configuration UI
```

Pregunta del siguiente chat:

1. ¿Qué primitives/composiciones UI transversales CURRENT ya existen y deben reutilizarse?
2. ¿Qué visualizaciones realmente desaparecieron frente a una versión histórica verificable?
3. ¿Qué código histórico sigue siendo útil sólo como referencia y qué contrato CURRENT debe preservar?
4. ¿Cuál es el primer módulo faltante que puede cerrarse como incremento aislado?
5. ¿Qué comportamiento pertenece al Manager shell y cuál al dominio concreto?

Reglas:

```text
no inventar UI desde memoria
no restaurar legacy
no crear adapters/shims
no forzar Users a Source/Projection
no fusionar Profiles/Users/Access por simetría
```

Estado:

```text
CONFIGURATION-UI-COMPOSITION-RECOVERY
PLANNED / NEXT
```

## OPEN — Navigation fallback para identidad no promovida

La entrada a la aplicación ya es CURRENT para identidad autenticada no promovida.

Sigue OPEN la composición exacta de `NavigationPrincipal`/perfil de fallback.

No resolver mediante:

```text
guest authority en Users
UserRecord ficticio
Navigation -> Users dependency
Navigation -> ADA Access dependency
```

## OPEN — Users Administration surface

```text
USERS-ADMINISTRATION-SURFACE-CUTOVER
PLANNED / SEPARATE
```

El core de administración existe. Falta superficie UI.

No reintroducir Users Source/Projection.

## OPEN — Profiles Configuration UI

Profiles core + configuration + Source lifecycle existen.

Falta editor/surface administrativa.

```text
PLANNED / SEPARATE INCREMENT
```

## OPEN — ADA Access Configuration UI

ADA Access core + configuration + Source lifecycle existen.

Falta editor/surface administrativa.

```text
PLANNED / SEPARATE INCREMENT
```

No confundir esta UI con el wiring runtime exacto de ADA Access.

## OPEN — ADA Access runtime composition

```text
OPEN / SEPARATE
```

No convertir ADA Access en dependency de Navigation.

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

## OPEN — Python package metadata alignment

Canonical fija Python 3.14.7.

Packages CURRENT aún contienen metadata 3.14.2.

```text
PYTHON-METADATA-ALIGNMENT
PLANNED / OPEN
```

## PLANNED — Web test contract cleanup

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN
```

No mezclar con UI recovery salvo bloqueo directo.

## UNVERIFIED

```text
visualizaciones históricas concretas reportadas como perdidas
full web pytest después del callback final
full ADA pytest después del callback final
CI remoto de 9f12c41...
full Ruff workspace
Python/Trixie global qualification
```
