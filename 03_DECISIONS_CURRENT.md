# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global

```text
Python 3.14.7
uv, no pip normal
contracts before consumers
backend before frontend
clean root cutover
no legacy adapters/shims/aliases
no double contract
one focus per increment
Git read-only unless explicit authorization
```

## Manager authorization

CURRENT / FROZEN:

```text
Manager authorization != ADA Access
```

Contrato genérico:

```text
manager_access_granted(principal, access_key)

access_key is None                      → DENY
administrative_override=True            → ALLOW
access_key in principal.access_keys     → ALLOW
otherwise                               → DENY
```

Manager Core no interpreta perfiles de ADA, Users, Profiles ni Navigation para decidir override.

`is_local` es contexto, no permiso.
`profile_keys` son contexto, no permiso automático.

Granular `access_keys` se conserva como contrato para administración delegada futura.

## ADA composition of Manager principal

CURRENT / FROZEN:

```text
managed root                         → administrative_override
trusted local + local environment    → administrative_override
basic / guest / custom               → no Manager administration
bootstrap root                       → no implicit Manager administration
```

La composición ADA es responsable de decidir el override; Manager Core permanece genérico.

`ManagerPrincipal.access_keys` no se rellena desde `AdaAccessConfiguration`.

## ADA Access boundary

CURRENT / FROZEN:

```text
ADA Access
profile_key → operational access_keys
```

No usar ADA Access como autoridad de Manager.

No crear persistencia duplicada de permisos Manager para resolver root/local.

Navigation, ADA Access y Manager mantienen semánticas de autorización independientes aunque una aplicación componga las tres.

## Local Manager runtime

CURRENT / FROZEN:

```text
local temporary principal
→ administrative_override=True
→ access_keys=()
```

No reconstruir listas exhaustivas de `*.manage`.

La constante agregada `MANAGER_ACCESS_KEYS` queda retirada.

## Tool persistence and runtime

CURRENT / FROZEN:

```text
Tool runtime
→ durable Tool Projection
```

Source participa en publicación/materialización, no es requisito para leer una Projection activa válida.

Los estados de resolución siguen separados:

```text
READY
UNCONFIGURED
UNAVAILABLE
INVALID
```

## KPI Collector

CURRENT / FROZEN:

```text
Latest polling      10 s default
Timeseries polling 120 s default
Browser refresh     10 s default
Latest priority
1 ToolComponent = 1 logical/browser store
Subcomponent != Store
browser cache only
```

Tool Projection y KPI Delivery pueden usar conexiones distintas.

## Operational render

CURRENT / FROZEN:

```text
CONFIGURATION DETERMINES STRUCTURE
DATA DETERMINES RUNTIME STATE
```

`OperationalRenderBinding` es estructural; no transporta estado KPI.

## Tooling next boundary

No existe una decisión nueva aprobada en este cierre sobre consolidación Tool→Tool.

CURRENT observado:

```text
ToolConfiguration.source_consumption
→ source_keys
```

OPEN / UNVERIFIED:

```text
semántica de una Tool que consolida/consume otra Tool
```

Regla para el siguiente chat:

```text
inspect implementation + decisions + canonical first
do not invent a tool dependency schema
do not encode another Tool as a source_key without an approved contract
```

## Decisiones anteriores reemplazadas o refinadas

```text
"root/local full Manager access by enumerating every *.manage key"
SUPERSEDED
```

Ahora se usa `administrative_override`.

```text
"ADA Access determines Manager permissions"
SUPERSEDED
```

ADA Access y Manager authorization quedan desacoplados.

```text
"Navigation administrative override is not a Manager permission"
REFINED
```

Navigation conserva su propia autorización, pero `ManagerPrincipal.administrative_override`
es ahora también parte explícita del contrato genérico de autorización Manager.
No confundir el efecto Manager con la semántica propia de Navigation.

## Próxima decisión

```text
ADA-TOOLING-CONTRACT-REVIEW
PLANNED / NEXT
```

No abrir KPI, Alarm, Command Center o distribución como refactors paralelos durante esa revisión.
