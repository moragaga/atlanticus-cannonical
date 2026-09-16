# Atlanticus — Roadmap

Estado: **CANONICAL BASELINE 1.0 — EXECUTION IN PROGRESS**

## Regla

Cerrar contratos raíz de forma limpia y luego integrar consumers.

Un solo foco por incremento.

No conservar legacy para mantener consumers o tests anteriores funcionando.

## Checkpoint publicado de referencia

```text
moragaga/atlanticus@ef3f0a44c5dcc14f8fcafe5bb36bb97865381924
```

## Hitos cerrados

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-MANAGER-GENERIC-CONTRACT-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-CLEAN-CUTOVER-COMPLETION
CLOSED / VERIFIED / CURRENT

USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL
CLOSED / VERIFIED / CURRENT

PROJECTION-CORE-STALE-TEST-ALIGNMENT
CLOSED / VERIFIED

TOOLS-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## Configuration domains closure

Los dominios relevantes para `ada-configuration-manager` ya tienen contrato final:

```text
Users                         CURRENT
Navigation                    CURRENT
Tools Source/Projection       CURRENT
KPI Configuration             CURRENT
KPI Definition                CURRENT
```

No queda otro dominio Configuration que deba migrarse antes del consumer final.

## Siguiente foco único

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
PLANNED / NEXT
```

Secuencia del siguiente chat:

1. inspeccionar únicamente `scopes/ada/web/application/ada-configuration-manager` y sus dependencias contractuales directas;
2. verificar todos los imports/contratos legacy que siguen presentes;
3. contrastar cada módulo con `ManagerModule` CURRENT y los Source/Projection services CURRENT de su dominio;
4. definir el wiring final completo antes de editar;
5. eliminar adapters/workflows cuya única función sea traducir revision strings o contratos legacy;
6. eliminar `KpiDefinitionAuthority` del consumer si sólo existe para sostener el contrato removido;
7. usar identidad local de workspace sólo donde corresponda al contrato Manager CURRENT;
8. transformar o eliminar tests directamente ligados a contratos SUPERSEDED;
9. no introducir aliases, shims, wrappers de compatibilidad ni doble routing;
10. no tocar Manager core ni dominios ya cerrados salvo conflicto real demostrado;
11. implementar sólo después de consenso explícito.

## Después del Configuration Manager

```text
MANAGER-CONSUMER-GLOBAL-QUALIFICATION
BLOCKED until final Manager consumer cutover
```

Después, en incremento separado:

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / AFTER MANAGER
```

Ese frente revisará tests Web cuya única finalidad sea congelar CSS, estructura interna o existencia/no existencia de funciones/clases.

## Open independiente

Existe una desalineación de metadata Python:

```text
canonical baseline: Python 3.14.7
KPI Configuration pyproject: requires-python ==3.14.2
KPI Definition pyproject:    requires-python ==3.14.2
```

No resolverla dentro del Configuration Manager final salvo que el usuario abra explícitamente ese frente o impida la qualification del incremento.

## No mezclar en el siguiente chat

- Python baseline cleanup;
- cleanup transversal de tests Web;
- Docker E2E general;
- Command Center;
- Operational Data;
- rediseño de Manager core;
- rediseño de Projection core;
- reintroducción de contratos legacy en dominios ya cerrados.

Único foco:

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
```
