# Web Platform — Current Gaps

Estado: **CURRENT**

Checkpoint de implementación:

```text
moragaga/atlanticus@4e008055ddc551e6c08a7d87715340c8c7cd149e
```

## 1. Users / Profiles / Navigation

### Cerrado

```text
USERS-STANDALONE-AUTHORITY-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-BOUNDARY-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Users core posee autoridades base:

```text
guest
basic
root
local
```

Profiles CURRENT tiene:

```text
profiles/core
profiles/configuration
```

`ProfilesConfiguration` es owned por `profiles/configuration`, no por core.

### Gap vigente

CURRENT todavía conserva ownership combinado dentro de Users configuration:

```text
UsersProfilesConfiguration
UsersProfilesAdministrationService
UsersProfilesAdminDraft
combined Users + Profiles Source
combined Users + Profiles Projection payload
profile_key in UserConfiguration
administrator in combined contract
Profiles UI inside Users UI
```

Siguiente frontera:

```text
USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER
PLANNED / NEXT
```

Después permanecen:

```text
USERS-PROFILES-COMPOSITION-CUTOVER
PROFILES-UI-EXTRACTION
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
```

## 2. Capability composition

Target vigente:

```text
Users
  ↓
Profiles
  ↓
Navigation
```

Gap:

- composition final no está implementada;
- cross-capability authority validation sigue pendiente;
- Navigation alignment sigue pendiente.

La regla anterior de Navigation standalone queda SUPERSEDED.

## 3. User Activity

### Existe

- event model;
- route changes;
- active time;
- route aggregates;
- Cosmos adapter;
- Memory adapter;
- Identity binding.

### Gap

No hay page visit history ordenada.

## 4. TTL

El contrato canónico requiere 24 h.

Debe verificarse dónde se declara físicamente el `CosmosContainerSpec` de User
Activity.

No asumir que TTL está aplicado sólo porque el dominio lo requiere.

## 5. Cosmos provisioning / Web lifecycle

Existe `CosmosProvisioner` y soporta:

- create database;
- ensure containers;
- validate containers;
- partition key validation;
- TTL validation.

Además permanecen cerrados:

```text
WEB-STORAGE-TOPOLOGY
USERS-STORAGE-TOPOLOGY
STORAGE-PREFLIGHT-COSMOS-BRIDGE
```

Gap vigente:

- integrar resource preparation al lifecycle Web;
- congelar `ApplicationResourcePlan`;
- required/optional semantics;
- named connection resolution global;
- política local/cloud de database creation;
- readiness READY/DEGRADED/ERROR.

## 6. Users runtime / local

Users runtime durable y Cosmos adapter permanecen CURRENT.

Existe selector local Jane/John en Users core.

Gap:

```text
composition/runtime wiring exacto del selector local
UNVERIFIED
```

No asumir que la existencia del selector prueba que el runtime ejecutado lo usa.

## 7. Profiles source / projection ownership

Todavía no existe lifecycle independiente completo de Profiles.

Target pendiente:

```text
source_key = profiles
Profiles Source owns ProfilesConfiguration
Profiles projection/configuration lifecycle owned by Profiles
```

No crear un segundo Source paralelo mientras el combined contract siga CURRENT.

El siguiente cutover debe reemplazarlo de raíz.

## 8. Storage provisioning

No se ha cerrado parity equivalente a Cosmos provisioning para toda
`connectivity/storage`.

Diseñar sólo si Source/Blob/bootstrap lo requiere.

## 9. Manager bypass

`is_local` full-access bypass continúa como open item donde aún corresponda.

No mezclarlo con Profiles capability extraction.

## 10. Pre-Manager page

La superficie final de bootstrap/login permanece pendiente según canonical
Manager.

## 11. Projection planner

Manager tiene workflows de proyección por módulo.

No introducir un coordinator global sin necesidad real demostrada.

## 12. Test hygiene

Existe un test CURRENT de Users core que inspecciona source/import absence.

Gap:

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN
```

No añadir tests nuevos de CSS visual, source tokens, imports, AST o estructura
interna.

## 13. Command Center

Debe aplicar los mismos principios de:

- capability ownership;
- resource bootstrap;
- readiness;
- projection orchestration;
- optional User Activity.

No transferir automáticamente contratos del Web Platform a Alarm Engine sin una
frontera explícita.
