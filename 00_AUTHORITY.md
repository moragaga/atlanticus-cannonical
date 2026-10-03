# Atlanticus — Authority

Estado: **CURRENT**

## Repositorios autoritativos auditados

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Commit auditado: `38bcd8c5607d67f999e2bc4bf9dbf176c8340588`
- Fecha del commit: `2026-10-03T01:09:03Z`

### Decisiones

- Repositorio: `moragaga/atlanticus-decisions`
- Rama: `main`
- Commit auditado: `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`
- Fecha del commit: `2026-09-13T03:31:11Z`

### Canonical antes de este reemplazo

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Commit inspeccionado: `0a2ff691d8bb9a4bfe743cadb1cacb9c83d865c7`
- Fecha del commit: `2026-10-03T01:26:18Z`

## Jerarquía

1. `atlanticus:main`: realidad implementada actual.
2. Decisión explícitamente vigente/frozen en `atlanticus-decisions:main`: intención contractual autoritativa, incluso cuando aún no esté implementada.
3. Qualification, tests y evidencia reproducible: propiedades demostradas.
4. Documentación canónica: estado vigente, decisiones del Project ya aceptadas y conflictos conocidos.
5. Decisiones recientes del Project todavía no formalizadas en `atlanticus-decisions`: pueden quedar `DECIDED / PLANNED`, pero nunca presentarse como implementación.
6. Historial conversacional: pista de búsqueda, no autoridad autónoma.

## Clasificación epistemológica

- `VERIFIED`
- `INFERRED`
- `ASSUMED`
- `PROPOSED`
- `UNVERIFIED`

## Estado conceptual

- `CURRENT`
- `IN PROGRESS`
- `PLANNED`
- `SUPERSEDED`
- `BLOCKED`
- `CLOSED`

## Git

Git es **SOLO LECTURA** por defecto.

No crear commits, push, ramas, PR, issues ni otra mutación remota sin autorización explícita.

Generar reemplazos canónicos, handoffs o ZIPs locales no modifica Git.

## Reglas de conflicto

No elegir silenciosamente entre implementación, decisiones y canonical.

Cuando una decisión aceptada todavía no esté implementada, registrar:

```text
DECIDED / PLANNED
implementation: CURRENT OLD CONTRACT
```

No introducir adapters, aliases, compatibilidad legacy o doble contrato para conservar el estado anterior durante un root cutover salvo autorización explícita.

## Conflictos globales previos preservados

Este cierre no reabre ni resuelve los siguientes frentes ya registrados:

### Alarm Materialization

```text
VERIFIED / CONFLICT / OPEN
```

Sigue pendiente reconciliar la decisión histórica que ubica adquisición/resolución dentro de Alarm Materialization con la dirección de Project que propone una configuración operacional autosuficiente publicada por Command Center.

### Ownership físico de Alarm Engine

```text
PROPOSED / OPEN
```

No ejecutar extracción física mientras la frontera contractual de materialización no quede reconciliada.

### Ownership físico de KPI Engine

```text
PROPOSED / OPEN
```

Permanece diferido y no forma parte del cutover ADA Tool/User.

## Conflicto focal CURRENT

### ADA Tool-scoped configuration y Users runtime

`VERIFIED / DECIDED / NOT YET IMPLEMENTED`

La implementación actual todavía mantiene `Navigation`, `Profiles`, `ADA Access` y `Operational` sobre el `application_prefix`, y `UserRecord` global todavía contiene `profile_key` y `enabled`.

El contrato aceptado para el siguiente incremento es:

```text
application namespace
    users identity registry only

tool namespace
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

`users-runtime` de Cosmos pertenece a una Tool y será el snapshot completo autoritativo para lectura de sesión de esa Tool.

La reconstrucción granular:

```text
Global Users
+ Tool Membership
+ Profiles
+ Operational
→ join por IDs
→ users-runtime
```

queda `PLANNED / FUTURE`, no bloquea el primer cutover.

No se identificó en `atlanticus-decisions:main` una decisión textual frozen que defina el contrato nuevo o que lo contradiga explícitamente. Hasta formalizarlo allí, canonical debe distinguir implementación actual de dirección decidida.
