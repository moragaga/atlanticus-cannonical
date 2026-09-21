# Web Platform — External Resource Requirements

Estado: **CONTRACT DESIGN / NAMED CONNECTION DIRECTION REFINED**

## Problema

La Web debe preparar recursos propios usados por Backend sin obligar al Backend a depender de Web.

También puede consumir recursos externos preexistentes que no son propiedad de la aplicación.

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

La Web/deployment composition importa esos specs, no el runtime del job.

## Managed vs external resources

No todo recurso referenciado por una aplicación debe ser provisionado por ella.

Deben distinguirse semánticamente:

### Managed application resource

Recurso cuya topología pertenece a la aplicación/deployment y puede participar en validate/ensure según política.

### External consumed resource

Recurso preexistente perteneciente a otro owner.

La aplicación consumidora:

- resuelve su conexión nombrada;
- valida lo necesario para consumir;
- no crea el database;
- no crea/modifica containers externos por defecto;
- no altera partitioning/TTL del owner externo;
- puede operar read-only cuando ese sea el contrato.

La forma exacta de representar esta distinción en `ApplicationResourcePlan` permanece abierta.

## Ejemplo

```text
Alarm Runtime package
   └── declares managed resources:
       alarms-live
       alarms-management
       ...

Command Center
   ├── declares its managed resources
   └── references external Tool Cosmos inputs as read-only dependencies

Web composition
   └── includes managed requirements in ApplicationResourcePlan
       and resolves external connection references without provisioning them
```

Esto permite:

```text
Web deploy first
→ managed resources ready
→ external dependencies checked/read as applicable
→ Alarm backend deploy later
```

sin:

```text
Alarm job iteration
→ ensure Cosmos container
```

## Ownership

Cada managed resource requirement debe indicar:

- owner;
- connection name;
- resource name;
- partitioning/TTL contract;
- required/optional;
- purpose.

Cada external consumed resource debe preservar:

- external owner;
- connection name/reference;
- purpose;
- required/optional/readiness semantics;
- access mode cuando sea contractualmente relevante.

## Named connections

El plan debe respetar conexiones nombradas.

No asumir:

```text
one global Cosmos
one global Storage
```

Una aplicación puede tener simultáneamente:

```text
command-center Cosmos
external Tool Cosmos A
external Tool Cosmos B
...
```

Cada conexión se resuelve de forma explícita por composición.

No introducir clientes globales compartidos accidentalmente entre conexiones.

## Command Center Tool Catalog

La dirección congelada para Command Center es:

```text
named external Tool Cosmos connections
→ read confirmed Tool projections
→ reconcile
→ durable Tool Catalog revision in Blob
```

No se crea un Cosmos adicional de Command Center sólo para volver a publicar el Tool Catalog consolidado.

## Seguridad

El plan no contiene secretos.

Sólo:

- nombres lógicos;
- contratos;
- referencias de conexión.

Credenciales siguen el mecanismo normal del proyecto.

Las conexiones externas deben recibir únicamente los permisos necesarios; para Tool Catalog la dirección es consumo read-only.
