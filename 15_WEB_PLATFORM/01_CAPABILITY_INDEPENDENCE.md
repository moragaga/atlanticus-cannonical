# Web Platform — Capability Independence

Estado: **CURRENT / REFINED**

## Regla

Independencia técnica de una capability no significa que toda combinación de
capabilities sea válida operacionalmente.

Separar:

```text
core dependency
```

de:

```text
composition requirement
```

Las integrations deben permanecer en composition/binding cuando no exista una
responsabilidad de dominio que justifique acoplar cores.

## Global Users

Users puede existir standalone y es independiente de una aplicación concreta.

```text
Global Users
VALID
```

Users CURRENT no es configuration Source.

Estructura:

```text
users/core
users/blob
users/cosmos
users/activity
```

Global User no contiene:

```text
profile_key
app role
Navigation configuration
Tools configuration
KPI configuration
ADA-specific Access
```

Strong identity:

```text
issuer + subject_id
```

## Profiles

Profiles es first-class capability y pertenece al plano application-specific.

CURRENT:

```text
profiles/core
profiles/configuration
```

El hecho de que una aplicación pueda asociar Profiles a Global Users no autoriza a
Profiles core a apropiarse del Users registry ni a Users a conocer todas las apps.

El contrato exacto de asociación todavía no está congelado.

Estado:

```text
OPEN / FUTURE INCREMENT
```

## Access

Access es application-specific y puede consumir Profiles/Users mediante composition.

No agregar Access al Global `UserRecord`.

El ownership concreto de la asociación User/Profile/Access permanece OPEN.

## Navigation

Navigation continúa siendo configuration domain independiente en core.

La regla anterior que representaba todo el target como:

```text
Users
  ↓
Profiles
  ↓
Navigation
```

queda **REFINED / SUPERSEDED AS COMPLETE CONTRACT**.

Motivo: Users ya no es Source/configuration y no debe convertirse en dependencia
directa de Navigation sólo para conservar el diagrama anterior.

Target conceptual vigente:

```text
Global Users registry
        │
        └── app composition resolves Profile/Access
                         │
                         └── Navigation consumes effective app authorization/profile data
```

La forma exacta del binding sigue PLANNED y debe derivarse de contratos CURRENT de
Profiles/Access, no inventarse en Navigation core.

## User Activity

User Activity conserva independencia funcional respecto de Users Administration,
Navigation y Manager salvo integrations explícitas.

Su dependencia mínima puede seguir siendo:

```text
Identity
+
Web runtime
```

Un binding de Navigation hacia Activity puede existir sin fusionar sus domains.

## Manager

Manager registra únicamente módulos de Configuration presentes en la composition.

No obliga por sí mismo a instalar:

```text
Users
Profiles
Navigation
Tools
KPI
Alarm
...
```

Users CURRENT no es `ManagerModule`.

Cada módulo administrativo conserva ownership propio.

## Invariante estructural

Cuando varias capabilities tienen la misma responsabilidad, usar el mismo concepto.

Para configuration domains que tengan ambas responsabilidades:

```text
<capability>/core
<capability>/configuration
```

No aplicar este patrón mecánicamente a Users: el package `users/configuration` fue
eliminado porque la responsabilidad no corresponde.

Para Profiles CURRENT:

```text
profiles/core
profiles/configuration
```

`profiles/management` no es parte del target.

## Dashboard

Dashboard puede unificar visualmente información de varias capabilities sin convertir
esa vista en dependencia de dominio.

```text
Users data ─────┐
Activity data ──┼──► Dashboard/read model
Navigation ─────┘
```

Los productores preservan ownership.

## Estado de implementación

En:

```text
moragaga/atlanticus@6dd09a6f24370bbad8ae358b6d5d7c6ea9aeba4a
```

están CLOSED/CURRENT:

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
PROFILES-CONFIGURATION-BOUNDARY-CUTOVER
```

Permanece IN PROGRESS:

```text
PROFILES-CAPABILITY-EXTRACTION
```

Siguiente Users focus:

```text
USERS-PERSISTED-DATA-CUTOVER
PLANNED / NEXT
```
