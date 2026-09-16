# Web Platform — Users / Profiles / Navigation Capability Boundary

Estado: **DECIDED / NOT YET IMPLEMENTED**

## Propósito

Este documento fija la frontera objetivo entre las capabilities genéricas:

```text
Users
Profiles
Navigation
```

y sirve como checkpoint canónico para un cutover que puede abarcar múltiples chats e incrementos.

No describe todavía realidad implementada salvo donde se marca explícitamente como `VERIFIED`.

## Autoridad de implementación

Baseline inspeccionado:

```text
moragaga/atlanticus:main
d3883e1e57ed6f4fdc88ae258b7b2f9ea1c99a9b
```

Parent:

```text
ee9a0401c7947f2bf61abc0a783dfa905443b6b1
```

Git permanece **SOLO LECTURA** para el asistente.

## Estado del incremento

```text
USERS-PROFILES-NAVIGATION-CAPABILITY-BOUNDARY
IN PROGRESS

PROFILES-CAPABILITY-EXTRACTION
PLANNED

USERS-STANDALONE-AUTHORITY-CUTOVER
PLANNED

USERS-PROFILES-COMPOSITION-CUTOVER
PLANNED

NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
PLANNED
```

Los nombres anteriores expresan frentes lógicos. No autorizan mezclar todos los cambios en un único incremento de implementación.

## Regla principal

Atlanticus es una base genérica.

Las capabilities deben tener ownership, lifecycle, configuración, UI y contratos propios cuando exista una responsabilidad funcional independiente.

El target de composición queda:

```text
Users
  │
  │ standalone válido
  ▼
Profiles
  │
  │ requiere Users
  ▼
Navigation
    requiere Profiles
    y por transitividad Users
```

Combinaciones válidas:

```text
Users
VALID

Users + Profiles
VALID

Users + Profiles + Navigation
VALID
```

Combinaciones inválidas:

```text
Profiles sin Users
INVALID

Navigation sin Profiles
INVALID

Users + Navigation sin Profiles
INVALID
```

## Capability Users

### Objetivo

Users debe poder existir como capability genérica standalone.

Por tanto:

```text
Users -> Profiles package dependency
REMOVE

Users -> Navigation dependency
FORBIDDEN
```

Users conserva identidad, observación, promoción, habilitación y runtime de usuarios.

Users no debe necesitar `ProfileDefinition`, `ProfileCatalog` ni otro modelo de Profiles para resolver un usuario standalone.

### Autoridades base

Users posee un contrato de autoridad mínimo independiente de Profiles:

```text
guest
basic
root
local
```

Estas claves no deben modelarse como `ProfileDefinition` dentro de Users.

Representan autoridad/runtime base de Users.

### guest

```text
TRANSITIONAL
NON-ASSIGNABLE
```

Semántica:

- autoridad inicial de un usuario nuevo observado;
- representa un usuario todavía no promovido;
- no puede seleccionarse ni asignarse manualmente;
- al promover al usuario debe abandonar el estado `guest`.

`guest` no es un perfil funcional configurable.

### basic

```text
ASSIGNABLE
STANDARD
```

Semántica:

- autoridad normal mínima de un usuario promovido;
- permite que Users funcione sin instalar Profiles;
- puede asignarse administrativamente;
- no implica acceso global.

Cuando Profiles está presente, un usuario `basic` puede posteriormente ser promovido a una autoridad/perfil funcional configurado por Profiles.

### root

```text
ASSIGNABLE
SYSTEM FULL AUTHORITY
```

Semántica:

- autoridad máxima del sistema;
- reemplaza completamente el concepto `administrator`;
- no pertenece a una aplicación concreta;
- es adecuada para una capability genérica.

Regla:

```text
administrator
REMOVE
```

No se debe conservar alias, shim ni compatibilidad `administrator -> root`.

### local

```text
LOCAL-RUNTIME ONLY
NON-ASSIGNABLE
FULL AUTHORITY
```

Semántica:

- exclusiva del despliegue/runtime local;
- nunca debe aparecer como opción asignable a usuarios administrados;
- posee acceso completo;
- representa la identidad local efectiva.

El runtime local alterna aleatoriamente entre dos identidades base:

```text
Jane Doe
authority = local
avatar background = #C85D91
avatar text       = #FFFFFF

John Doe
authority = local
avatar background = #3778C2
avatar text       = #FFFFFF
```

Jane y John son usuarios/identidades locales, no Profiles independientes.

Los colores pertenecen a la identidad/avatar local y no requieren crear `local-jane` o `local-john`.

## Capability Profiles

### Ownership

Profiles es una capability Web genérica de primer nivel:

```text
web/capabilities/profiles/
```

Debe tener responsabilidad propia para:

```text
domain/core
configuration
source
administration
UI
lifecycle
```

donde cada package exista sólo si hay una frontera técnica o de responsabilidad real.

Profiles no debe continuar administrado desde:

```text
web/capabilities/users/configuration/
```

### Dependencia

Profiles requiere Users.

```text
Profiles -> Users
REQUIRED
```

La razón es funcional: Profiles extiende la autoridad de usuarios promovidos y no tiene utilidad operacional standalone dentro del modelo Atlanticus definido.

Esto no autoriza a Profiles a apropiarse de identidad, pending users, runtime store o lifecycle de Users.

### Perfiles funcionales

Profiles agrega autoridades/perfiles funcionales configurables sobre el contrato base de Users.

Ejemplos no canónicos:

```text
operator
viewer
engineer
planner
```

No son system profiles implícitos.

La configuración real determina cuáles existen.

### Asignabilidad

Autoridades base asignables sin Profiles:

```text
basic
root
```

Reservadas / no asignables:

```text
guest
local
```

Cuando Profiles está activo, el universo asignable se amplía:

```text
basic
root
+ configured functional profiles
```

`guest` y `local` permanecen no asignables.

## Capability Navigation

Navigation necesita Profiles en la composición Atlanticus definida por este contrato.

Por transitividad:

```text
Navigation
    ↓
Profiles
    ↓
Users
```

Esto significa que no se considera una composición válida:

```text
Navigation standalone
```

ni:

```text
Users + Navigation
```

sin Profiles.

### Boundary técnico

La dependencia funcional anterior no obliga automáticamente a que `navigation/core` importe clases concretas de Profiles.

Debe preferirse un contrato desacoplado de autorización, por ejemplo una clave de acceso efectiva:

```text
principal.access_key
```

comparada contra las claves admitidas por una ruta.

La integración Users + Profiles debe producir la autoridad efectiva que Navigation consume.

No introducir dependencia concreta entre cores salvo que la implementación demuestre que existe una responsabilidad contractual que no pueda mantenerse en composition/binding.

## Estado CURRENT verificado

En `atlanticus@d3883e1e...`:

### Users

`atlanticus-web-users` declara actualmente una dependencia directa:

```text
atlanticus-web-profiles
```

`UsersAccessResolver` requiere actualmente:

```text
ProfileCatalog
```

y los usuarios resueltos requieren `profile_key`.

Por tanto:

```text
Users standalone
NOT CURRENT
```

### Profiles

Existe:

```text
web/capabilities/profiles/core
```

pero CURRENT sólo contiene el core de definición/configuración.

`ProfilesConfiguration` acepta actualmente un catálogo vacío.

No existe todavía el lifecycle independiente completo de Profiles definido en este documento.

### Administración combinada

CURRENT contiene:

```text
UsersProfilesConfiguration
UsersProfilesAdministrationService
UsersProfilesAdminDraft
```

y la administración de Users posee operaciones de Profiles.

También existe publicación conjunta Users + Profiles bajo el source de Users.

Todo ello contradice el target de ownership separado.

### administrator

CURRENT crea un perfil default:

```text
administrator
background = #673AB7
text       = #FFFFFF
```

Este contrato queda:

```text
SUPERSEDED BY DECISION
```

pero todavía no está removido de la implementación.

### guest / local

CURRENT conserva semántica parcial de `guest`.

La historia verificada de Atlanticus demuestra que existieron perfiles/autoridades de sistema `local`, `administrator` y `guest`, además de los colores de Jane y John.

Este documento reemplaza `administrator` por `root` como decisión contractual actual.

## Evidencia histórica recuperada

En un checkpoint histórico inspeccionado de Atlanticus existían:

```text
local
background = #3778C2
text       = #FFFFFF

guest
background = #FF5722
text       = #FFFFFF

administrator
background = #673AB7
text       = #FFFFFF

John Doe local avatar
background = #3778C2
text       = #FFFFFF

Jane Doe local avatar
background = #C85D91
text       = #FFFFFF
```

Los valores anteriores son evidencia histórica útil.

No convierten automáticamente `administrator` en contrato vigente.

## Contratos combinados a eliminar

El target final no conserva ownership combinado Users/Profiles.

```text
UsersProfilesConfiguration
REMOVE

UsersProfilesAdministrationService
REMOVE

UsersProfilesAdminDraft
REMOVE

Profiles source inside Users source
REMOVE

Profiles UI inside Users UI
REMOVE

administrator
REMOVE
```

No introducir:

```text
legacy wrappers
aliases
compatibility shims
dual contracts
temporary administrator mapping
distributed transaction Users + Profiles
```

## Publicación y consistencia entre Users y Profiles

Separar Users y Profiles implica perder la publicación atómica actualmente obtenida por el agregado conjunto.

No recrear esa atomicidad mediante transacción distribuida.

Principio:

```text
cada capability publica su propio source
```

La integridad entre ambas debe resolverse mediante contratos explícitos de composición y secuencias operacionales verificables.

Ejemplo de una futura eliminación de un perfil funcional referenciado:

```text
1. identificar usuarios referenciando el perfil
2. reasignarlos a una autoridad válida
3. publicar Users
4. eliminar el perfil
5. publicar Profiles
```

El detalle exacto de recovery, retry y auditoría debe definirse antes de implementar ese flujo.

## Source / Projection

Target conceptual:

```text
Users
source_key = users

Profiles
source_key = profiles

Navigation
source_key = navigation
```

Cada capability debe usar los contratos genéricos Atlanticus de Source / Projection / Manager cuando corresponda.

No crear una arquitectura especial para Users, Profiles o Navigation.

## UI

Users y Profiles son pantallas diferentes con flujos propios.

Target:

```text
Users UI
ownership = Users

Profiles UI
ownership = Profiles

Navigation UI
ownership = Navigation
```

Profiles UI no debe permanecer embebida como una sección administrativa de Users.

La composición puede presentarlas dentro del mismo Manager sin fusionar ownership.

## Conflicto con canonical CURRENT

`15_WEB_PLATFORM/01_CAPABILITY_INDEPENDENCE.md` dice actualmente que:

```text
Navigation puede existir sin Users/Profile
Navigation sin Profile integration funciona sin filtros de perfil
profile-navigation binding es opcional
```

Eso contradice la decisión actual:

```text
Navigation requires Profiles
Profiles requires Users
```

Por tanto:

```text
15_WEB_PLATFORM/01_CAPABILITY_INDEPENDENCE.md
PARTIAL CONFLICT
```

Al integrar este documento en canonical, las reglas anteriores de independencia de Navigation deben marcarse `SUPERSEDED` o actualizarse explícitamente.

No resolver este conflicto silenciosamente.

## Reglas congeladas para el cutover

```text
Atlanticus generic
REQUIRED

Users standalone
REQUIRED

Users -> Profiles dependency
REMOVE

Users -> Navigation dependency
FORBIDDEN

Profiles first-class capability
REQUIRED

Profiles -> Users
REQUIRED

Navigation -> Profiles
REQUIRED AT COMPOSITION CONTRACT

Navigation -> Users direct ownership
FORBIDDEN

guest
TRANSITIONAL / NON-ASSIGNABLE

basic
ASSIGNABLE / STANDARD

root
ASSIGNABLE / FULL AUTHORITY

local
LOCAL-RUNTIME ONLY / NON-ASSIGNABLE / FULL AUTHORITY

administrator
REMOVE

Jane Doe local identity/colors
PRESERVE

John Doe local identity/colors
PRESERVE

Profiles UI inside Users
REMOVE

UsersProfiles aggregate contracts
REMOVE

LEGACY
REMOVE

ADAPTERS / SHIMS / ALIASES
FORBIDDEN

DOUBLE CONTRACT
FORBIDDEN
```

## Orden de implementación propuesto

El trabajo debe avanzar incrementalmente y con un foco único por incremento.

Orden recomendado:

```text
1. USERS-STANDALONE-AUTHORITY-CUTOVER
   - definir autoridad base Users
   - guest/basic/root/local
   - remover dependencia obligatoria Users -> Profiles
   - preservar runtime local Jane/John

2. PROFILES-CAPABILITY-EXTRACTION
   - mover ownership restante de Profiles fuera de Users
   - crear source/admin/lifecycle propio
   - remover UsersProfiles aggregate

3. USERS-PROFILES-COMPOSITION-CUTOVER
   - resolver promoción/asignación y validación cross-capability
   - definir reglas operacionales de referencias

4. PROFILES-UI-EXTRACTION
   - pantalla Profiles independiente
   - Users UI sólo administra Users

5. NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
   - alinear composición Navigation => Profiles => Users
   - preservar core desacoplado cuando sea posible
   - revisar `allowed_profiles` vs contrato genérico de access key sólo si el cutover lo exige

6. QUALIFICATION
   - comportamiento
   - invariantes
   - regresiones
   - composición válida/inválida
   - source/projection
   - local runtime
```

No ampliar un incremento únicamente para dejar todos los consumidores verdes mediante compatibilidad temporal.

Un consumer puede quedar temporalmente roto durante un root cutover si el contrato anterior se elimina de raíz y el siguiente incremento tiene ownership claro.

## Criterios de aceptación finales

El cutover completo queda cerrado sólo cuando:

```text
Users funciona sin Profiles
Profiles tiene lifecycle propio y requiere Users
Navigation se compone sólo con Profiles + Users
administrator no existe
guest no es asignable
local no es asignable
basic es asignable
root es asignable y full authority
Jane/John local se preservan con sus colores
Profiles UI está fuera de Users
no existe UsersProfiles aggregate
no existe source Users+Profiles combinado
no existen shims/aliases/adapters legacy
tests verifican comportamiento e invariantes finales
```

## Pendientes de verificación durante implementación

```text
- localizar todos los consumidores CURRENT de UsersProfilesConfiguration
- localizar todos los consumidores CURRENT de UsersProfilesAdministrationService
- trazar source/projection combined Users+Profiles
- verificar el runtime local CURRENT y dónde se selecciona Jane/John
- definir representación exacta de authority_key en Users
- definir cómo Profiles extiende authority_key sin apropiarse de Users
- definir migración de documentos CURRENT que todavía contienen administrator
- definir recovery/auditoría para referencias Users -> functional profile
- resolver explícitamente el conflicto con 15_WEB_PLATFORM/01_CAPABILITY_INDEPENDENCE.md
```

Hasta que cada punto se verifique:

```text
NO ASSUMPTIONS
NO SILENT COMPATIBILITY
NO SILENT CANONICAL CONFLICT RESOLUTION
```
