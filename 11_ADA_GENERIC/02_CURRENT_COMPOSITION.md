# ADA Generic — Current Composition

Estado: **CURRENT / CORE STAGE 1 CLOSED / MANAGER AUTHORIZATION CONVERGED / TOOLING REVIEW NEXT**

Corte inspeccionado:

```text
moragaga/atlanticus@6fd1512afed73e76f7c344f3acb989b601c453e3
```

## Composition CURRENT

ADA Generic compone capabilities independientes y consume Tool Projection durable.

```text
AdaGenericSettings
→ ToolPersistenceComposition
→ resolve_operational_tool_projection()
→ READY | UNCONFIGURED | UNAVAILABLE | INVALID
→ ADA Generic Web
```

Runtime ordinario no necesita Tool Source cuando existe Projection válida.

Con Tool READY y KPI Delivery configurado puede adjuntar `AdaKpiCollector`.

`OperationalRenderBinding` conserva estructura; no transporta KPI state.

## Manager + Identity CURRENT

Manager se integra al disponer de stores/dependencies.

Autenticación, ADA Access, Navigation y Manager authorization son fronteras distintas.

Composición del principal Manager:

```text
managed root
→ administrative_override=True

trusted local + local environment
→ administrative_override=True

basic / guest / custom / unknown
→ no administrative override

bootstrap root
→ no implicit Manager administration
```

`ManagerPrincipal.access_keys` permanece vacío en estas rutas.

ADA Generic ya no lee `AdaAccessConfiguration` ni `ProfileCatalog` para derivar permisos Manager.

El Configuration Manager local usa override en lugar de una lista agregada de permisos.

## Manager authorization CURRENT

```text
manager_access_granted(principal, access_key)

None          → DENY
override      → ALLOW
granular key  → ALLOW
otherwise     → DENY
```

Los `can_manage` internos de ADA Configuration Manager usan la misma semántica.

## Qualification observada

```text
ADA Generic application       288 passed
ADA Generic ruff              PASS
ADA Configuration Manager      70 passed
Atlanticus Manager Core        85 passed
MANAGER_ACCESS_KEYS search      0 matches
```

## Tooling CURRENT observado

`ToolConfiguration` contiene:

```text
tool_key
display_name
kind
source_consumption
source_operational_participation
structure
branding
```

`ToolSourceConsumption` representa:

```text
tool_key
source_keys
```

Kinds observados:

```text
integrated_operations
process
strategic
```

`ToolStructure` entrega estructura reutilizada por KPI y por contratos de Alarm baseline.

## Frontera no cerrada

No se demostró todavía un contrato explícito para consolidación:

```text
Tool A → Tool B
```

No asumir si B consume Source originales, una Projection de A, una publicación de A o algún contrato distinto.

No inventar ese contrato.

## Siguiente foco

```text
ADA-TOOLING-CONTRACT-REVIEW
```

Primero auditar implementación, decisions y canonical.
Luego definir si existe gap real.

KPI E2E, Alarm E2E, login/bootstrap físico y distribución final se validan después sobre el contrato Tooling confirmado.
