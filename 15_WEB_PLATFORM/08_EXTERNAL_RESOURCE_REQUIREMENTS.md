# Web Platform — External Resource Requirements

Estado: **CONTRACT DESIGN**

## Problema

La Web debe preparar contenedores usados por Backend sin obligar al Backend a depender de Web.

No introducir:

```text
backend package
→ imports atlanticus.web
```

## Dirección

Los owners de runtime declaran requisitos mediante contratos neutrales.

Ejemplo conceptual:

```text
BackendResourceRequirements
├── owner_key
├── cosmos[]
├── storage[]
└── required_for[]
```

La Web/deployment composition importa esos **specs**, no el runtime del job.

## Ejemplo

```text
Alarm Runtime package
   └── declares:
       alarms-live
       alarms-management
       ...

ADA Web composition
   └── includes Alarm resource requirements
       in ApplicationResourcePlan
```

Esto permite:

```text
Web deploy first
→ resources ready
→ Alarm backend deploy later
```

sin:

```text
Alarm job iteration
→ ensure Cosmos container
```

## Ownership

Cada resource requirement debe indicar:

- owner;
- connection name;
- resource name;
- partitioning/TTL contract;
- required/optional;
- purpose.

## Named connections

El plan debe respetar conexiones nombradas.

No asumir:

```text
one global Cosmos
one global Storage
```

## Seguridad

El plan no contiene secretos.

Sólo:

- nombres lógicos;
- contratos;
- referencias de conexión.

Credenciales siguen el mecanismo normal del proyecto.
