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

## Users

Users debe poder existir standalone.

```text
Users
VALID
```

Users no requiere Profiles ni Navigation para identidad, pending/resolved
runtime y autoridades base.

Users puede depender de Identity según el contrato de autenticación vigente.

## Profiles

Profiles es first-class capability y extiende el universo de autoridad funcional
de Users.

Composition vigente:

```text
Users + Profiles
VALID

Profiles without Users
INVALID
```

La dependencia funcional `Profiles => Users` no obliga automáticamente a que
`profiles/core` importe clases de Users.

Debe mantenerse ownership separado.

## Navigation

Target Atlanticus vigente:

```text
Users + Profiles + Navigation
VALID

Navigation without Profiles
INVALID

Users + Navigation without Profiles
INVALID
```

La regla anterior:

```text
Navigation standalone
optional profile-navigation binding
```

queda:

```text
SUPERSEDED
```

### Boundary técnico

Navigation core debe permanecer desacoplado cuando sea posible.

Preferir consumo de una autoridad efectiva:

```text
principal.access_key
```

contra claves configuradas/admitidas, en vez de importar modelos concretos de
Profiles dentro del core.

La composition Users + Profiles produce la autoridad efectiva que Navigation
consume.

## User Activity

User Activity conserva independencia funcional respecto de Users Configuration,
Navigation y Manager salvo integrations explícitas.

Su dependencia mínima puede seguir siendo:

```text
Identity
+
Web runtime
```

Un binding de Navigation hacia Activity puede existir sin fusionar sus domains.

## Manager

Manager registra únicamente módulos presentes en la composition.

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

Cada módulo administrativo conserva ownership propio.

## Invariante estructural

Cuando varias capabilities tienen la misma responsabilidad, usar el mismo
concepto:

```text
<capability>/core
<capability>/configuration
```

No crear nombres especiales sin frontera real.

Para Profiles CURRENT:

```text
profiles/core
profiles/configuration
```

`profiles/management` no es parte del target.

## Dashboard

Dashboard puede unificar visualmente información de varias capabilities sin
convertir esa vista en dependencia de dominio.

```text
Users data ─────┐
Activity data ──┼──► Dashboard/read model
Navigation ─────┘
```

Los productores preservan ownership.

## Estado de implementación

En:

```text
moragaga/atlanticus@4e008055ddc551e6c08a7d87715340c8c7cd149e
```

están CLOSED/CURRENT:

```text
USERS-STANDALONE-AUTHORITY-CUTOVER
PROFILES-CONFIGURATION-BOUNDARY-CUTOVER
```

La composition final Users/Profiles/Navigation sigue:

```text
IN PROGRESS / PLANNED BY INCREMENT
```
