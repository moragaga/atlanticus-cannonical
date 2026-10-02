# Atlanticus — Authority

Estado: **CURRENT — cierre Dual App Durable + Master Projection 2026-10-02**

## Referencias verificadas para este cierre

```text
Implementation evidence checkpoint
moragaga/atlanticus:main
7bd11afdf2af82c56fb100f4aa5336c039d9bd22

Decisions
moragaga/atlanticus-decisions:main
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e

Canonical leído antes de estos reemplazos
moragaga/atlanticus-cannonical:main
cedbe3156bc384add0de58cda27f2b77628c133d
```

El SHA de implementación es evidencia del estado validado en este hito, no un freeze global:
`main` puede avanzar por frentes paralelos. Revalidar únicamente rutas, contratos y dependencias
que intersecten el siguiente incremento.

## Jerarquía

1. `moragaga/atlanticus:main` es la realidad implementada.
2. Una decisión explícitamente vigente/frozen en `moragaga/atlanticus-decisions:main` define
   intención contractual. Si contradice implementación, registrar `CONFLICT`.
3. `moragaga/atlanticus-cannonical:main` describe el estado vigente y debe mantenerse
   sincronizado con implementación y decisiones.
4. Qualification/tests/logs sólo acreditan el alcance realmente ejecutado.
5. Historial conversacional es pista de búsqueda, nunca autoridad suficiente.

No resolver contradicciones silenciosamente.

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

Web CURRENT:

```text
Python == 3.14.2
```

Objetivo histórico:

```text
Python 3.14.7
python:3.14.7-slim-trixie
```

La migración permanece:

```text
PLANNED / DEFERRED
```

No es blocker para Source, runtime dual, Master Projection ni distribución actual.

## Siguiente frontera única

```text
SOURCE-NAMESPACE-AND-COMPOSITION-CONVERGENCE
PLANNED / NEXT
```

El objetivo es conservar `SourceStore` y sus providers genéricos y corregir ownership/composición
consumida por ADA y ADA Command Center, comenzando por el acoplamiento actual:

```text
ada-command-center
    -> ada.web.storage.namespace
```

No abrir durante ese incremento:

```text
tooling topology reorganization
KPI / Collector / UI
production Entra
Python/Trixie migration
```
