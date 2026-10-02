# Atlanticus — Authority

Estado: **CURRENT**

## Repositorios autoritativos auditados

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Commit auditado: `0e7db853802dc59ffd85410a611cdc093b08dc20`
- Fecha del commit: `2026-10-02T19:10:34Z`

### Decisiones

- Repositorio: `moragaga/atlanticus-decisions`
- Rama: `main`
- Commit auditado: `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`
- Fecha del commit: `2026-09-13T03:31:11Z`

## Jerarquía

1. `atlanticus:main`: realidad implementada actual.
2. Decisión explícitamente vigente/frozen en `atlanticus-decisions:main`: intención contractual autoritativa, incluso cuando la implementación aún no la materializa.
3. Qualification, tests y evidencia reproducible: propiedades demostradas.
4. Documentación canónica del Project: estado vigente, conflictos conocidos y delta pendiente de formalización.
5. Decisiones recientes del Project aún no formalizadas en `atlanticus-decisions`: dirección propuesta o acordada localmente, pero no reemplazan silenciosamente una decisión frozen incompatible.
6. Memoria e historial conversacional: pista de búsqueda, nunca autoridad suficiente por sí sola.

## Clasificación epistemológica

Usar siempre una de estas etiquetas cuando exista incertidumbre relevante:

- `VERIFIED`
- `INFERRED`
- `ASSUMED`
- `PROPOSED`
- `UNVERIFIED`

## Estado conceptual

Usar siempre una de estas etiquetas para estado de trabajo:

- `CURRENT`
- `IN PROGRESS`
- `PLANNED`
- `SUPERSEDED`
- `BLOCKED`
- `CLOSED`

## Conflictos

No elegir silenciosamente entre implementación, decisiones y canónico.

Clasificar explícitamente como corresponda:

- `IMPLEMENTED + VALIDATED`
- `DECIDED / NOT YET IMPLEMENTED`
- `CURRENT`
- `SUPERSEDED`
- `HISTORICAL`
- `DRAFT`
- `DUPLICATE`
- `CONFLICT`
- `UNVERIFIED`

Un cambio de ownership físico, dirección de dependencias o responsabilidad entre productos debe tratarse como decisión arquitectónica. Mover código sin reconciliar una decisión frozen incompatible no está permitido.

## Git

Git es **SOLO LECTURA** por defecto.

No crear commits, push, ramas, PR, issues ni otra mutación remota sin autorización explícita del usuario.

La generación local de archivos de propuesta, reemplazos canónicos o ZIPs integrables no modifica Git y es válida cuando el usuario la solicita.

## Referencias externas

Otros repositorios, proyectos, implementaciones o versiones solo son referencias cuando el usuario lo indique. No transfieren automáticamente autoridad, contratos, nombres, dependencias ni arquitectura a Atlanticus.

## Conflictos vigentes relevantes para el próximo hito

### Alarm Materialization

`VERIFIED / CONFLICT / OPEN`

La decisión registrada de R3.6M-006B.2 ubica en Alarm Materialization la adquisición del candidato, la lectura de Alarm Configuration y Confirmed Tool Catalog, y la resolución B.2.

La implementación actual refleja esa dirección y contiene dependencias desde backend/materialization hacia paquetes bajo `web`.

La dirección reafirmada en el Project para el próximo hito propone que Command Center resuelva y publique una configuración operacional autosuficiente y que Alarm Materialization solo la consuma y produzca los artefactos de Runtime y Delivery.

Hasta que se formalice el reemplazo correspondiente en `atlanticus-decisions`, este punto debe permanecer explícitamente como `CONFLICT` y no resolverse de forma implícita.

### Ownership físico de Alarm Engine

`PROPOSED / OPEN`

La decisión histórica fija `scopes/ada-command-center/backend/` como scope físico de Alarm Engine.

La implementación ha madurado hacia un conjunto autónomo de `core`, `materialization`, `persistence` y procesos de runtime/materialization/delivery. Se propone evaluar su extracción a un scope propio de engine, pero no debe ejecutarse antes de cerrar la frontera contractual anterior.

### Ownership físico de KPI Engine

`PROPOSED / OPEN`

KPI ya presenta responsabilidades de engine separadas (`core`, `evaluation`, `persistence`, `history`, `delivery` y procesos asociados), aunque actualmente viva bajo `scopes/ada/backend`.

Su extracción física y la introducción de una materialización equivalente a Alarm quedan planificadas para un hito posterior, sin mezclarlo con la corrección inmediata de Alarm Materialization.
