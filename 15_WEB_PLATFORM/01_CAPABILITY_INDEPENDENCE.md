# Web Platform — Capability Independence

Estado: **CURRENT DIRECTION**

## Regla

Una capability Web no debe requerir otra capability funcional no esencial para poder existir.

La aplicación decide qué módulos integra.

Objetivo:

```text
Identity
   │
   ├──────────────┐
   ▼              ▼
Users/Profile   User Activity

Navigation

Manager

otras capabilities
```

Las capacidades anteriores pueden combinarse, pero la integración ocurre en **composition/binding packages**, no introduciendo dependencias cruzadas en sus cores.

## Invariantes

### Users / Profiles

Debe poder existir sin:

- Navigation;
- User Activity;
- Manager;
- Tool Configuration.

Puede depender de Identity porque resuelve una identidad autenticada hacia un usuario/perfil efectivo.

### Navigation

Debe poder existir sin:

- Users/Profile;
- User Activity;
- Manager.

Si no existe Profile integration:

```text
Navigation
→ funciona sin filtros de perfil
```

Si se desea filtrar por perfil:

```text
Users/Profile
      +
Navigation
      ↓
optional profile-navigation binding
```

### User Activity

Debe poder existir sin:

- Users Configuration;
- Navigation;
- Manager.

Su dependencia mínima puede ser:

```text
Identity
+
Web runtime
```

Si Navigation está instalada, un binding opcional puede traducir pathname hacia una identidad semántica de ruta.

### Manager

Debe registrar únicamente los módulos presentes en la composición.

No debe obligar a instalar:

```text
Users
Navigation
Tools
KPI
Alarm
...
```

para existir.

Cada módulo administrativo se incorpora de forma independiente.

## Precedente verificado

Atlanticus ya contiene:

```text
atlanticus-web-composition-navigation-activity
```

Ese package combina Navigation + User Activity sin acoplar sus cores.

Este patrón se convierte en referencia para futuras integraciones.

## Gap actual ADA Manager

La composición actual de ADA Manager construye opciones de perfiles para Navigation consultando directamente:

```text
dependencies.users.administration.load_catalog()
```

Por tanto:

```text
ADA Manager Navigation
        ↓
conoce Users
```

Esto no rompe el core genérico de Navigation, pero sí acopla la composición ADA.

Objetivo:

```text
Navigation module
        │
        ├── standalone
        │
        └── + optional profile binding
                    ↓
                Users/Profile
```

La eliminación de este coupling debe hacerse como incremento aislado, preservando comportamiento.

## Dashboard

El dashboard puede **unificar visualmente** información proveniente de varias capabilities.

No debe convertir esa unificación en dependencia de dominio.

```text
Users data ─────┐
Activity data ──┼──► Dashboard/read model
Navigation ─────┘
```

Los productores permanecen independientes y el dashboard conserva el origen de cada dato.
