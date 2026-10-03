# Atlanticus — Architecture

Estado: **CURRENT + DECIDED NEXT CUTOVER**

## Regla principal

Atlanticus es una plataforma modular reusable.

ADA y ADA Command Center son consumidores. El núcleo genérico de Atlanticus no depende de ADA.

## Generic Web architecture preserved

Reusable capabilities remain under Atlanticus generic ownership:

```text
Source Core / Local / Blob
Projection Core
Storage Namespace
Storage Topology
Users
Profiles
Navigation
Manager
Master Projection
```

Product composition remains responsible for selecting and connecting those capabilities.

```text
ADA Generic
    product composition root

ADA Command Center Generic
    separate product composition root
```

The reusable Master Projection engine remains generic; products own their projection-domain composition/provisioning.

## Environment versus persistence

Frozen:

```text
ATLANTICUS_ENVIRONMENT
    host/runtime behavior

persistence mode
    local | durable
```

Emulator versus Azure is connection configuration, not an architecture mode.

## Storage Namespace

Contrato generic CURRENT:

```text
StorageNamespace(
    application_namespace,
    scope_namespace,
)

application_prefix = <application_namespace>
scope_prefix       = <application_namespace>/<scope_namespace>
```

Para ADA:

```text
application_namespace = conciencia_situacional
scope_namespace       = ADA_TOOL_NAMESPACE
```

## ADA ownership objetivo aceptado

### Application-global

Sólo información realmente compartida por todas las Tools de una misma aplicación.

CURRENT target:

```text
Global Users identity registry
```

El usuario global representa identidad y atributos personales/globales. No contiene estado de pertenencia a una Tool.

### Tool-scoped

Toda configuración que puede variar entre Tools vive bajo `scope_prefix`.

```text
Tool Configuration
Profiles
Navigation
ADA Access
Operational
Tool User Membership
KPI Registry
KPI Definitions
Tool Users Recovery Snapshot
```

No introducir un tercer nivel artificial `tools/`; `scope_prefix` ya expresa:

```text
conciencia_situacional/<tool>
```

## Cosmos

Supuesto de infraestructura aceptado para ADA actual:

```text
one Cosmos database/runtime deployment per Tool
```

No agregar ahora protección multi-tool intra-Cosmos, particionamiento adicional o routing complejo sólo para escenarios que la infraestructura no usa.

Cosmos sigue siendo superficie de proyección/consumo.

## Users

### Global durable identity

Target:

```text
user_id
issuer
subject_id
display_name
email
```

`profile_key` y `enabled` salen del contrato global.

### Tool User Membership

Nuevo contrato Tool-scoped:

```text
user_id
profile_key
enabled
```

Representa pertenencia y estado del usuario dentro de una Tool.

### users-runtime

`users-runtime` en Cosmos es un snapshot denormalizado y completo para la sesión de esa Tool.

Incluye:

```text
identity
enabled
resolved profile
resolved operational data
```

Operational conserva shape estable aunque no exista información:

```text
area.id/label       = null
position.id/label   = null
group.id/label      = null
```

El runtime no debe consultar Blob para resolver sesión.

## Access

No duplicar `access_keys` dentro del snapshot de cada usuario.

Mantener:

```text
profile_key -> access_keys
```

como proyección/caché pequeña de Access.

## Recovery

CURRENT immediate target:

```text
Tool Users Recovery Snapshot
→ users-runtime
```

FUTURE / PLANNED:

```text
Global Users
+ Tool Membership
+ Profiles
+ Operational
→ join por IDs
→ users-runtime
```

No implementar esta reconstrucción granular en el próximo incremento.

## Navigation authorization target

Semántica aceptada:

```text
PUBLIC
RESTRICTED
```

`RESTRICTED` con cero perfiles ordinarios significa sólo `root/local`.

`RESTRICTED` con perfiles significa esos perfiles más `root/local`.

Root/local permanecen privilegiados implícitos y no grants editables.

## KPI Registry / Delivery

KPI Registry Projection/materialization debe transportar el `tool_key` estable derivado de Tool Projection.

Delivery consume `tool_key`; `display_name` sigue siendo presentación de Tool Configuration, no identidad contractual KPI.

## Tooling topology preserved

```text
/tooling
    reusable/transversal mechanisms

/scopes/ada/tooling
    ADA-specific distribution composition

/scopes/ada-command-center/tooling
    Command Center-specific distribution composition
```

Process Distribution remains separate from Web Distribution.

## Clean cutover rule

Al implementar este cambio:

```text
replace root contracts cleanly
no legacy adapters
no aliases
no dual old/new user model
no compatibility storage paths
```
