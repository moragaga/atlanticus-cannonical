# Manager — Source Ledger

Estado: **AUDIT LEDGER**

## Autoridad

- `moragaga/atlanticus:main` = realidad implementada publicada.
- `moragaga/atlanticus-cannonical:main` = autoridad documental vigente.
- `moragaga/atlanticus-decisions` = HISTORICAL.
- Git permanece SOLO LECTURA para el asistente.

## Checkpoint publicado de este cierre

```text
moragaga/atlanticus@9f12c41a23d69784c7c5b775a4093a94ac654d55
```

Parent:

```text
3eb46dac80f23d438774e3afa39999dc96f592d7
```

Tree:

```text
dd002b632b494065428af9dd10f1e58b7e6638d1
```

## Manager authorization semantics

```text
MANAGER-AUTHORIZATION-SEMANTICS-ALIGNMENT
CLOSED / VERIFIED / CURRENT
```

Implementado:

```text
ManagerModuleAccess REMOVED
ManagerModule.access_key CURRENT
ManagerAuthorizationPolicy.can_view CURRENT
Default authorization = explicit access key membership
is_local bypass REMOVED
administrator profile bypass REMOVED
per-operation validate/publish/project Manager permissions REMOVED
```

ADA Configuration Manager:

```text
navigation.manage
tools.manage
kpis.manage
```

Local runtime recibe esas capabilities explícitamente.

## Manager active workflow callback

```text
MANAGER-ACTIVE-WORKFLOW-CALLBACK-CARDINALITY
CLOSED / VERIFIED / CURRENT
```

Problema reproducido:

```text
InvalidCallbackReturnValue
Expected 1, got 0
```

Fix:

```text
unresolvable/non-visible active module
→ PreventUpdate
```

Targeted regression + manual smoke posterior: PASS observado.

## Dependency alignment incidental necesaria

ADA Configuration Manager fue alineado a:

```text
atlanticus-web-navigation-configuration[web]==0.1.9
```

Su lock fue actualizado y `uv lock --check` pasó.

## Qualification observada

Durante el hito:

```text
legacy scan scoped                         0
Manager + navigation-manager tests         68 PASS
web full pytest before final callback fix  PASS / 7 skipped
ADA Configuration Manager pytest           26 PASS
focused Ruff/format                        PASS
callbacks commented mirror AST             PASS
callback targeted regression               PASS
manual /manager smoke after final fix       PASS
HTTP 500 after final fix                    not observed
InvalidCallbackReturnValue after final fix  not observed
```

No declarar full web/ADA pytest rerun después del delta final del callback.

## Finding descubierto durante cierre documental

```text
ManagerAuthorizationPolicy.can_view(...)
```

vs:

```text
web/compositions/navigation-manager
resolved_authorization.can_access(...)
```

Estado:

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

El consumer no fue demostrado por el smoke ADA Configuration Manager.

## UI CURRENT observada

Configuration Manager compone:

```text
navigation
tools
kpis
kpi-definitions
```

No compone actualmente:

```text
Profiles Configuration
Users Administration
ADA Access Configuration
```

El backend/lifecycle de esos dominios existe en sus fronteras actuales; la ausencia es de
surface/composition, no una autorización para reconstruir dominios.

## Qualification pendiente

```text
full web pytest después del delta final
UNVERIFIED

full ADA pytest después del delta final
UNVERIFIED

full Ruff workspace
UNVERIFIED

CI remote
UNVERIFIED

Storage/Cosmos E2E
UNVERIFIED
```

## Próxima frontera

```text
CONFIGURATION-UI-COMPOSITION-RECOVERY
PLANNED / NEXT
```
