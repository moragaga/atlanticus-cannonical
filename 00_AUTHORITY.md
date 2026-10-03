# Atlanticus — Authority

Estado: **CURRENT**

## Repositorios autoritativos auditados

### Implementación
- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Commit auditado: `2f9b65c3ba2646d519abfb0bb49e095d6819d185`
- Fecha del commit: `2026-10-03T05:22:45Z`
- Mensaje: `feat`

### Decisiones
- Repositorio: `moragaga/atlanticus-decisions`
- Rama: `main`
- Commit auditado: `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`
- Fecha del commit: `2026-09-13T03:31:11Z`

### Canonical antes de este reemplazo
- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Commit inspeccionado: `c530eec42e792ed9dc8aef4efbc07a0b94d6f1c9`
- Fecha del commit: `2026-10-03T01:49:32Z`

## Jerarquía

1. `atlanticus:main`: realidad implementada actual.
2. Decisión explícitamente vigente/frozen en `atlanticus-decisions:main`: intención contractual autoritativa.
3. Qualification, tests y evidencia reproducible.
4. Canonical vigente.
5. Decisiones recientes del Project aún no formalizadas en decisions: `DECIDED / PLANNED`, nunca implementación si `main` no las contiene.
6. Historial conversacional: pista, no autoridad.

## Git

Git continúa **SOLO LECTURA** para el asistente salvo autorización explícita.

## Estado focal de este cierre

### ADA Tool-scoped configuration + Users runtime

```text
VERIFIED / CURRENT / CLOSED
```

`atlanticus:main@2f9b65c3ba2646d519abfb0bb49e095d6819d185` contiene el cutover limpio:

```text
application-global
    Users identity registry

Tool-scoped Blob
    Navigation Source
    Profiles Source
    ADA Access Source
    Operational Source
    Tool Configuration Source
    Tool User Membership
    KPI Registry Source
    KPI Definitions Source
    Tool Users Recovery artifacts

Tool-owned Cosmos
    users-runtime
```

Global Users ya no posee `profile_key` ni `enabled`.

`users-runtime` contiene `RuntimeUser` materializado y, como Cosmos pertenece a una Tool, no repite `application_key` ni `tool_key`.

### Navigation access contract

```text
DECIDED / PLANNED / OPEN
implementation: CURRENT OLD SEMANTICS
```

CURRENT:

```text
allowed_profiles=[]
→ unrestricted/public
```

Target aceptado:

```text
PUBLIC
RESTRICTED + []
RESTRICTED + [profiles]
```

## Conflictos globales preservados

```text
Alarm Materialization boundary                    OPEN / separate
Alarm Engine physical ownership                   PROPOSED / separate
KPI Engine physical ownership                     PROPOSED / separate
Python 3.14.7 / Trixie migration                  PLANNED / separate
production Azure / Entra                          UNVERIFIED / separate
```
