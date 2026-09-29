# ADA Command Center — Domain Ownership and Migration

Estado: **CURRENT / ALARM DOMAIN + TOOLS DOMAIN IMPLEMENTED**

## Alarm authored domain

```text
scopes/ada-command-center/domain/alarms
ada-command-center-alarms-domain==1.0.0
```

Owns Alarm authoring contracts and `AlarmConfigurationSnapshot`.

## Command Center Tools shared domain

```text
scopes/ada-command-center/domain/tools
ada-command-center-tools-domain==1.0.0
```

Owns:

```text
ToolDependencyEntry
ToolDependencyManifest
```

This transversal contract is shared by Alarm publication/history and backend Materialization.

## Dependency direction CURRENT

```text
ada-web-tools structural contracts
        ↓
domain/tools
        ↓
domain/alarms snapshot wrapper
```

`domain/alarms` is no longer dependency-free.

The previous canonical statement `dependencies = []` is SUPERSEDED.

## Deferred normalization

Current Tools structural authority still lives under `ada-web-tools`.

Project decision:

```text
DOMAIN/TOOLS NORMALIZATION
PLANNED / DEFERRED
```

Do not refactor during Alarm Cosmos Projection unless it becomes a blocker.

## Materialization owner

```text
scopes/ada-command-center/backend/alarms/materialization
```

Owns pure resolution contracts/resolver, not stores/acquisition/orchestration.

## Process owner — CURRENT (la etiqueta histórica «planned» quedó SUPERSEDED)

```text
scopes/ada-command-center/backend/processes/alarms-materialization
```

Existe en `atlanticus:main` (materialization process B2c.7 ya auditado en el corte Engine). Su distribución física sigue el gate separado Engine/Delivery.

## Source/projection ownership

Alarm Configuration Web owns:
- Source codec/workflow;
- workspace correlation;
- base projection builder.

Next boundary is durable Cosmos Projection storage for `AlarmConfigurationSnapshot`.

## Límite Web B1d y composición distribuible

**CURRENT:** Domain Tools contiene el manifest transversal; Backend Tool Catalog/Discovery poseen consolidación/persistencia/adapter; `web/alarms/configuration` es capability separada. La UI de Tool Catalog aún reside en la aplicación temporal `ada-command-center-configuration-manager`. Este ownership Web no equivale a trasladar contratos Tool estructurales a Domain automáticamente: la normalización transversal `ada-web-tools` continúa diferida.

**DECIDED / PLANNED:** nueva biblioteca `web/tools/...` propietaria exclusivamente de la UI y la integración Manager de Tool Catalog; consumidores son el host actual (por sustitución sin legacy) y luego `ada-command-center-generic`/Starter. Aplicaciones ensamblan servicios; no se añade dependencia inversa desde Domain/Backend a Web o a ADA Generic. El Starter sólo instala dependencias opcionales de identidad/usuarios/perfiles/navigation cuando un contrato concreto las necesite.

## No legacy

No aliases/shims for:
- source schema v2;
- prior snapshot shape;
- old duplicate authored models.
