# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

## Autoridad

```text
Implementation
moragaga/atlanticus@6fd1512afed73e76f7c344f3acb989b601c453e3

Historical decisions
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e

Canonical inspected before replacement
moragaga/atlanticus-cannonical@2b6ef68cdbea6f1278ed60b9ba65485cec136d7d
```

Git permanece **SOLO LECTURA** durante este cierre.

## Estado resumido

```text
MANAGER-ADMINISTRATIVE-OVERRIDE                 CLOSED / VERIFIED / CURRENT
ADA-MANAGER-AUTHORIZATION-CONVERGENCE           CLOSED / VERIFIED / CURRENT
ADA-MANAGER-PRINCIPAL-DECOUPLING                CLOSED / VERIFIED / CURRENT
ADA-ACCESS-AND-MANAGER-SEPARATION               CLOSED / VERIFIED / CURRENT
ADA-GENERIC-AUTHORIZATION-QUALIFICATION          CLOSED / VERIFIED / CURRENT

ADA-TOOLING-CONTRACT-REVIEW                     PLANNED / NEXT
ADA-END-TO-END-GOLDEN-PATH                      PLANNED / AFTER TOOLING REVIEW
ADA-LOGIN-DATA-BOOTSTRAP-E2E                    PLANNED / PART OF GOLDEN PATH
ADA-TOOL-CONSOLIDATION-CONTRACT                 OPEN / UNVERIFIED
ADA-KPI-END-TO-END-CONSUMPTION                  OPEN / UNVERIFIED
ADA-ALARM-END-TO-END-CONSUMPTION                OPEN / SEPARATE INTEGRATION
```

## Hito cerrado — autorización administrativa del Manager

Atlanticus Manager incorpora:

```text
ManagerPrincipal.administrative_override
manager_access_granted(principal, access_key)
```

Contrato implementado:

```text
access_key is None                      → DENY
principal.administrative_override=True  → ALLOW
access_key in principal.access_keys     → ALLOW
otherwise                               → DENY
```

Manager Core no infiere el override desde `profile_keys` ni desde `is_local`.

`is_local=True` por sí solo no concede acceso administrativo.
`profile_keys=('root',)` por sí solo no concede acceso administrativo.
`access_key=None` sigue denegado incluso con override.

## ADA — composición CURRENT

ADA Generic dejó de derivar permisos Manager desde `AdaAccessConfiguration`.

Composición vigente:

```text
managed root + authenticated EffectiveUser
→ ManagerPrincipal(administrative_override=True)

trusted local identity + local environment
→ ManagerPrincipal(administrative_override=True, is_local=True)

basic / guest / custom / unknown
→ no Manager administration

bootstrap_root without managed user
→ no implicit Manager administration
```

`ManagerPrincipal.access_keys` queda vacío en estas rutas de composición.

ADA Access conserva su ownership propio:

```text
profile_key → operational access_keys
```

y no se usa como autoridad de permisos administrativos del Manager.

El runtime local del Configuration Manager usa `administrative_override=True` y ya no mantiene una lista exhaustiva de permisos Manager.

`MANAGER_ACCESS_KEYS` fue retirado; la búsqueda final sobre `scopes/ada/web/application` y `web` reportó cero coincidencias.

## Qualification observada

Sobre el working tree que luego fue publicado en `6fd1512afed73e76f7c344f3acb989b601c453e3`:

```text
ADA Generic application       288 passed
ADA Generic ruff              PASS
ADA Configuration Manager      70 passed
Atlanticus Manager Core        85 passed
MANAGER_ACCESS_KEYS search      0 matches
git diff --check               PASS
```

Estas cifras corresponden al cierre de este hito; no deben extrapolarse a todo el monorepo.

## Tooling — estado observado, no rediseñado

Código CURRENT inspeccionado:

```text
ToolConfiguration
├── tool_key
├── display_name
├── kind
├── source_consumption
├── source_operational_participation
├── structure
└── branding

ToolSourceConsumption
└── source_keys

ToolConfigurationKind
├── integrated_operations
├── process
└── strategic
```

`ToolStructure` ya expone destinos KPI y contratos usados por la proyección baseline de Alarm.

No se demostró en este cierre un contrato explícito y cerrado para:

```text
Tool A
→ ser consumida / consolidada por
Tool B
```

No inventar schema, dependencia o adapter para resolverlo.

## Próxima frontera

Foco único recomendado:

```text
ADA-TOOLING-CONTRACT-REVIEW
```

Objetivo: auditar contratos e implementación CURRENT de Tooling, decisions y canonical antes de modificar código.

Después, y sólo después:

```text
ADA-END-TO-END-GOLDEN-PATH
```

con una instancia limpia que recorra configuración, identidad, Tool, KPI, Alarm e integración/distribución según contratos realmente existentes.
