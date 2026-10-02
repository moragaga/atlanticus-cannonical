# Atlanticus — Authority

Estado: **CURRENT — cierre Distribution/Web Tooling 2026-10-02**

## Referencias verificadas para este cierre

```text
Implementation
moragaga/atlanticus:main
2dc5862f634eb0bf8fe72d771d56605d1c7f32cf

Decisions
moragaga/atlanticus-decisions:main
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e

Canonical leído antes de estos reemplazos
moragaga/atlanticus-cannonical:main
852e031d020edd4fdd5ab0187e95fbf6f443083b
```

## Jerarquía

1. `moragaga/atlanticus:main` es la realidad implementada.
2. Una decisión explícitamente vigente/frozen en `moragaga/atlanticus-decisions:main` define intención contractual. Si contradice implementación, registrar `CONFLICT`.
3. `moragaga/atlanticus-cannonical:main` describe el estado vigente y debe mantenerse sincronizado con implementación y decisions.
4. Qualification/tests/logs sólo acreditan el alcance realmente ejecutado.
5. Historial conversacional es pista de búsqueda, nunca autoridad suficiente.

No resolver silenciosamente contradicciones.

Usar:

```text
VERIFIED
INFERRED
ASSUMED
PROPOSED
UNVERIFIED

CURRENT
IN PROGRESS
PLANNED
SUPERSEDED
BLOCKED
CLOSED
```

## Git

Git es **SOLO LECTURA** por defecto.

No crear commits, push, ramas, PR, issues ni mutaciones remotas sin autorización explícita.

## Python / imagen base

Estado CURRENT observado en Web distribution:

```text
Python == 3.14.2
```

Objetivo histórico del Project:

```text
Python 3.14.7
python:3.14.7-slim-trixie
```

La migración permanece:

```text
BLOCKED / DEFERRED UNTIL EXPLICIT USER AUTHORIZATION
```

No mezclarla con configuración, Master Projection, Collector o UI.

## Frontera Distribution/Web Tooling CURRENT

El motor compartido vive en:

```text
tooling/distribution/web/
```

y contiene sólo responsabilidades genéricas de:

```text
product catalog
starter generation
wheelhouse build
qualification/probe
distribution orchestration
base starter
```

Las superficies específicas de producto pertenecen a sus scopes:

```text
scopes/ada/tooling/distribution/web/
scopes/ada-command-center/tooling/distribution/web/
```

El runtime ADA y Master Projection no pertenecen al tooling.

## Siguiente frontera única

```text
ADA-AND-COMMAND-CENTER-ENV-DETAIL-CONTRACT
PLANNED / NEXT
```

Objetivo del siguiente chat:

1. auditar los `.env.detail` de ADA Generic y ADA Command Center;
2. definir valores manuales, derivados, secretos, locales y productivos;
3. congelar el contrato para `Storage durable/final + Cosmos local`;
4. congelar el contrato de Master Projection para ambas aplicaciones;
5. no levantar aún KPI/Collector/UI dentro del mismo incremento.

Después de cerrar este contrato se levantarán ambas aplicaciones. Luego el foco cambia exclusivamente a ADA para KPI/Collector/UI.
