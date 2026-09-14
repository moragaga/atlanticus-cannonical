# Atlanticus — Architecture

Estado: **CURRENT**

## Regla principal

Atlanticus es plataforma modular reusable.

ADA consume Atlanticus.

El núcleo genérico de Atlanticus no depende de ADA.

## Planos principales

### Platform

Capacidades transversales:
- backend;
- connectivity;
- integrations;
- web.

`backend/` representa backend jobs y capacidades propias de esos jobs.

`web/` es frontera de primer nivel para:
- Flask/Dash;
- JavaScript/CSS;
- composición Web;
- server-side Python cuya responsabilidad es Web;
- capabilities Web reutilizables.

Connectivity es dual-use y no adquiere ownership funcional.

### Configuration / Administration

Manager administra configuración, authoring, validation, publication, history y projection actions.

Source genérico pertenece a:

```text
web/capabilities/source/
```

Projection genérica exact-release pertenece a:

```text
web/capabilities/projection/core
```

### Operational Data

Operational Data conserva ownership separado para sources, producers, processes, planner y materialization.

### ADA Runtime

ADA Generic compone la experiencia operacional y consume capacidades Atlanticus.

ADA-specific authorization puede consumir/extender contratos genéricos, pero no convertirse en dependencia del core Atlanticus.

## Manager vs ADA Generic

```text
Manager      = administrar configuración
ADA Generic  = consumir configuración y materializar experiencia operacional
```

No comparten ownership de shell/header.

## Configuration vs Data

```text
CONFIGURATION DETERMINES EXISTENCE
DATA DETERMINES STATE
```

## Source vs Projection

Source y Projection son responsabilidades separadas.

```text
Source     = Local | Blob
Projection = Local | Cosmos
```

Projection representa un `SourceReleaseRef` concreto.

Source current nunca se determina desde Cosmos.

## Web Capability Composition

Las capabilities mantienen ownership separado y dependencias explícitas.

### Profiles / Users

Arquitectura CURRENT:

```text
Profiles
   ↑
   │ one-way dependency
Users
```

Contratos:
- Profiles vive en `web/capabilities/profiles/core`;
- Profiles no depende de Users;
- Profiles no depende de ADA;
- Users puede consumir Profiles;
- Users Configuration puede consumir contratos Profile;
- cross-capability binding pertenece a composición/adapters;
- `atlanticus.web.users.profiles` no existe como namespace productivo;
- no existe shim del namespace anterior.

### Profiles semántico

Profiles core modela exclusivamente Profiles funcionales explícitos.

```text
ProfileCatalog()
→ empty
```

No posee semántica especial de:
- Root;
- Guest/Pending;
- Local;
- John/Jane.

Administrator es un Profile funcional explícito cuando la proyección/configuración lo materializa.

### Pending / Guest

Pending pertenece a Users:

```text
PendingUserRecord
→ EffectiveUser(
     pending=True,
     profile=None,
     enabled=True,
     is_local=False
  )
```

Pending no depende de `ProfileCatalog`.

Guest no es un Profile runtime.

### Root bootstrap

Root pertenece a Identity/bootstrap:

```text
AuthenticatedIdentity
        ↓
BootstrapRootAccessResolver
        ├─ exact issuer + subject_id + enabled
        │      ↓
        │   READY
        │   bootstrap_root=True
        │   user_id=None
        │
        └─ otherwise
               ↓
          fallback AccessResolver
```

Root no se materializa como:
- User;
- Profile;
- nuevo `AccessStatus`.

El bootstrap Root genérico no equivale a política de autorización funcional ADA.

ADA Access puede consumir/extender Profiles y el contexto Identity, pero Profiles no depende de ADA Access.

### Local development identities

Local/John/Jane quedan fuera de Profiles.

Su representación runtime final permanece una frontera posterior.

## Users durable vs Profiles runtime

El aggregate durable `UsersConfigurationCatalog` aún contiene campos históricos de Administrator/Guest.

Eso no convierte esos campos en ownership de Profiles core.

La traducción runtime vigente produce:
- Administrator;
- Profiles funcionales configurados;
- no Guest;
- no Local.

Separar físicamente/durablemente los contratos Users y Profiles pertenece a `USERS-CONTRACT-SEPARATION`.

## Service Registry ownership

Una capability sólo registra servicios que le pertenecen.

CURRENT:

```text
create_users_module(runtime)
→ USERS_RUNTIME_SERVICE_KEY
```

Users no publica un ProfileCatalog mediante un service key propio.

Un consumidor que requiere `ProfileCatalog` debe recibirlo por composición explícita donde corresponda.

## Storage Resource Topology

Cadena vigente:

```text
Capability resource declaration
        ↓
StorageResourceContract[TTopology]
        ↓
resolve_storage_plan(...)
        ↓
ResolvedStorageResource[TTopology]
        ↓
provider bridge
        ↓
Connectivity primitive
        ↓
provision / validate
```

Storage Topology:
- no contiene secretos;
- no construye SDK clients;
- no hace I/O;
- resuelve conflicts/bindings antes del provider.

`users.runtime` permanece el único recurso durable Users confirmado.

No se crea `profiles.runtime` por inferencia.

## Source / Projection exact-release

Contrato:

```text
ProjectionTarget = SourceKey + SourceReleaseRef
```

`project(target)`:
- lee la release exacta;
- no relee current;
- conserva release identity;
- permite retry del mismo target.

No introducir adaptadores `SourceReleaseId <-> str`.

## Principio de implementación

Definir contratos antes que consumidores.

Backend antes que frontend cuando el contrato pertenece al backend; no usar esta regla para ubicar incorrectamente server-side Web.

Si una solución raíz reemplaza un contrato anterior, hacer cutover limpio:
- sin shims temporales;
- sin re-exports legacy;
- sin ownership duplicado.

## Fronteras futuras

No están cerradas por `PROFILES-BASELINE-SEMANTICS`:
- separación durable Users/Profiles;
- Profiles Source/Projection si se justifica;
- runtime canonical cutover Users;
- exact-release provenance en `users.runtime`;
- Root physical configuration;
- Local/John/Jane runtime contract;
- Admin composition conjunta sin recombinar ownership.

## Alarm Engine

Alarm conserva sus fronteras e invariantes cerrados.

No reabrir Alarm para resolver problemas de Users/Profiles/Identity.
