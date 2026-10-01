# Atlanticus — Authority

Estado: **CURRENT — cierre de auditoría transversal Manager / Web / Command Center 2026-10-01**

## Referencias verificadas para este cierre

```text
Implementation
moragaga/atlanticus:main
a75465745e188da4765e803595b17acaa55d9306

Decisions
moragaga/atlanticus-decisions:main
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e

Canonical leído antes de estos reemplazos
moragaga/atlanticus-cannonical:main
bb57b9d0e96e65014ef9042bed15bb4d47bfad58
```

Los reemplazos de este cierre se preparan contra esos tres cortes. Antes de integrarlos debe comprobarse que los archivos de destino no hayan cambiado.

## Jerarquía de autoridad

1. `moragaga/atlanticus:main` es la realidad implementada.
2. Una decisión explícitamente vigente/frozen en `moragaga/atlanticus-decisions:main` define intención contractual; si contradice implementación, registrar `CONFLICT`.
3. `moragaga/atlanticus-cannonical:main` describe el estado vigente y debe mantenerse sincronizado con implementación y decisions.
4. Qualification/tests/logs sólo acreditan el alcance realmente ejecutado.
5. Historial conversacional es pista de búsqueda, no autoridad suficiente.

No resolver silenciosamente contradicciones.

Usar siempre:

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

## Frontera temporal de Python e imagen base

Estado operativo CURRENT observado en los paquetes Web/Manager/Starter relevantes:

```text
Python == 3.14.2
```

Objetivo histórico del Project:

```text
Python 3.14.7
python:3.14.7-slim-trixie
```

Decisión explícita del usuario en este cierre:

```text
PYTHON-3.14.7-TRIXIE-MIGRATION
BLOCKED / DEFERRED UNTIL EXPLICIT USER AUTHORIZATION
```

Por lo tanto:

- no modificar `.python-version`, `requires-python`, locks, Dockerfiles o imágenes base para migrar a 3.14.7/Trixie;
- no usar la diferencia 3.14.2 vs 3.14.7 como motivo para bloquear o reabrir un incremento no relacionado;
- no ejecutar revisiones repetitivas cuyo único objetivo sea volver a señalar esa diferencia;
- conservar la discrepancia como objetivo histórico conocido, no como trabajo activo;
- sólo reabrir la migración cuando el usuario lo autorice expresamente.

## Manager — autoridad única

El package owner del Manager genérico es:

```text
web/capabilities/manager
package: atlanticus-web-manager
version CURRENT observada en a7546574: 0.3.18
```

`web/capabilities/manager/pyproject.toml` es la autoridad de versión del Manager.

No crear versiones independientes de Manager en ADA, ADA Command Center, Starters o tooling.

El siguiente frente debe converger todos los consumidores sobre una sola autoridad. Si la corrección del contrato reusable exige un bump, el bump se decide en el package owner y luego se propaga a todos los consumidores; no se inventa un número local en cada aplicación.

## Siguiente frontera única

```text
MANAGER-COMPOSITION-CONVERGENCE-AND-DUAL-PRODUCT-INTEGRATION
PLANNED / NEXT
```

El mismo chat debe:

1. cerrar el contrato reusable del Manager y sus compositions, con Navigation como divergencia principal;
2. hacer que ADA Generic consuma esa autoridad;
3. hacer que ADA Command Center consuma la misma autoridad;
4. alinear locks, manifests, generación/distribución y tooling de ambos productos con esa única autoridad;
5. calificar el recorrido integrado antes de abrir Resource Preparation u otros frentes.

No crear una segunda composición paralela para Command Center.

No dejar adapters legacy, aliases o contratos dobles para conservar implementaciones reemplazadas.
