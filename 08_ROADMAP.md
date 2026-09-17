# Atlanticus — Roadmap

Estado: **CANONICAL BASELINE 1.0 — EXECUTION IN PROGRESS**

## Regla

Un solo foco por incremento.

Cerrar cada frontera con evidencia suficiente para su alcance.

No conservar legacy para sostener consumers o tests anteriores.

No mezclar cleanup transversal con el incremento funcional activo.

## Checkpoint publicado de referencia

```text
moragaga/atlanticus@4e008055ddc551e6c08a7d87715340c8c7cd149e
```

Parent:

```text
709cf2fb9ee422094f011cfda051f08f37276992
```

## Hitos cerrados recientes

```text
USERS-STANDALONE-AUTHORITY-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-BOUNDARY-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Los hitos de Manager, Navigation generic configuration, Users legacy removal,
Tools, KPI Configuration, KPI Definition y ADA Configuration Manager cerrados
anteriormente permanecen CURRENT.

## Hito activo

```text
PROFILES-CAPABILITY-EXTRACTION
IN PROGRESS
```

CURRENT parcial:

```text
profiles/core
CURRENT

profiles/configuration
CURRENT

ProfilesConfiguration ownership
MOVED TO profiles/configuration
```

Todavía no está cerrado el lifecycle independiente completo de Profiles.

## Siguiente foco único

```text
USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER
PLANNED / NEXT
```

Objetivo contractual ya decidido:

```text
Users Source
UsersConfiguration only

Profiles Source
ProfilesConfiguration only

UsersProfilesConfiguration
REMOVE

UserConfiguration.profile_key
REMOVE

UserConfiguration.authority_key
FINAL

administrator
REMOVE from combined boundary
```

No crear aliases, adapters, shims ni compatibilidad entre contratos viejo/nuevo.

No inventar una arquitectura especial para Profiles.

## Después del Source ownership cutover

```text
USERS-PROFILES-COMPOSITION-CUTOVER
PLANNED
```

Debe resolver únicamente integración cross-capability:

```text
assignable authority universe
functional profile existence validation
referential operation sequencing
recovery/audit where required
```

No recrear transacción distribuida.

## Después de composition

```text
PROFILES-UI-EXTRACTION
PLANNED
```

Target:

```text
Users UI
Users-owned

Profiles UI
Profiles-owned
```

La apariencia se valida visualmente. No crear CSS-structure tests.

## Después de Profiles UI

```text
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
PLANNED
```

Target de composition:

```text
Navigation
  ↓
Profiles
  ↓
Users
```

Preservar core decoupling cuando la integración pueda vivir en composition.

## Qualification final del frente

```text
QUALIFICATION
PLANNED
```

Debe cubrir comportamiento, invariantes, regresiones y compositions
válidas/inválidas.

## Open independiente

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN

Python 3.14.7 metadata alignment
PLANNED / OPEN

CI remote
UNVERIFIED
```

Existe un test CURRENT que inspecciona source/import absence. No mezclar su
cleanup con el siguiente Source ownership cutover.

## No mezclar en el siguiente chat

- Profiles UI;
- Navigation alignment;
- E2E transversal;
- Python baseline cleanup;
- CSS visual tests;
- Web test cleanup global;
- Command Center;
- Operational Data;
- rediseño de Manager core;
- rediseño de Source/Projection core.

Único foco:

```text
USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER
```
