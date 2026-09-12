# Atlanticus — Authority

## Repositorios congelados para este bootstrap

- Implementación: `moragaga/atlanticus`
- Rama: `main`
- Commit auditado: `685924322c9cc0d625d112e25297a407f7a46acb`
- Fecha del commit: `2026-09-12T00:58:54Z`

- Decisiones: `moragaga/atlanticus-decisions`
- Rama: `main`
- Commit auditado: `ae28a733f9a973026182068af630a82ce39416bd`
- Fecha del commit: `2026-09-10T23:39:42Z`

## Jerarquía

1. `atlanticus:main`: realidad implementada.
2. Decisión explícitamente vigente/frozen en `atlanticus-decisions`: intención contractual, incluso si aún no está materializada.
3. Qualification y tests: evidencia de propiedades ya demostradas.
4. Decisiones recientes del Project: delta todavía no formalizado.
5. Memoria/historial conversacional: pista de búsqueda, nunca autoridad suficiente por sí sola.

## Conflictos

No elegir silenciosamente entre código y decisión. Clasificar como:

- `IMPLEMENTED + VALIDATED`
- `DECIDED / NOT YET IMPLEMENTED`
- `CURRENT`
- `SUPERSEDED`
- `HISTORICAL`
- `DRAFT`
- `DUPLICATE`
- `CONFLICT`
- `UNVERIFIED`

## Git

Git es **READ ONLY** por defecto. No crear commits, push, ramas, PR, issues ni mutaciones remotas sin autorización explícita.

## Referencias externas

Otros repositorios, proyectos o implementaciones solo son referencias cuando el usuario lo indique. No transfieren autoridad, contratos, nombres, dependencias ni arquitectura a Atlanticus.
