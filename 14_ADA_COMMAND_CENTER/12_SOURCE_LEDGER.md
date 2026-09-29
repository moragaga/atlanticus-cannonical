# ADA Command Center — Source Ledger

Estado: **AUDIT LEDGER — historia conservada y nuevo delta C1 Web verificado en 2026-09-29**. El título de la siguiente sección histórica «Current implementation» es histórico; sus SHAs **no** describen el HEAD actual.

## Current implementation — checkpoint histórico

```text
moragaga/atlanticus@880cb692054c2cd78cdc29cf62ab7b16bbd2c3d6
```

Relevant closure commits:

```text
9b9600ae96c9153cf70d0fb401905963b8583c2f
-> Command Center domain/tools + ToolDependencyManifest

d2a5e14822d3711e64668b8e70cfa15d7ddae2f0
-> AlarmConfigurationSnapshot v3
-> Tool manifest persistence
-> workspace Tool revision pin
-> validation/publication drift guard
-> local runtime integration
```

Los commits entre `d2a5e148...` y `880cb692...` pertenecían a otro frente de operational-data en aquel corte.

## Canonical base del checkpoint histórico

```text
moragaga/atlanticus-cannonical@148b178df74ee3083681140f3bb7997a02435b80
```

## Decisions históricas

```text
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

## Qualification histórica

```text
domain/tools             8 passed
domain/alarms           50 passed
web/alarms/configuration 35 passed
configuration-manager   11 passed
```

Lint y format gates GREEN según ese hito; no atribuir estas cifras a C1.

## Cambios contractuales históricos

**Added:** `scopes/ada-command-center/domain/tools`, `ToolDependencyEntry`, `ToolDependencyManifest`.

**Alarm Source anterior:**

```text
AlarmConfigurationSnapshot
    configuration
    confirmed_tool_catalog_revision
```

**CURRENT:**

```text
AlarmConfigurationSnapshot
    configuration
    tool_dependencies
```

`confirmed_tool_catalog_revision` se deriva. Source schema v2 **SUPERSEDED**, v3 **CURRENT**, sin decoder legacy v2.

**Workspace:** sidecar `_confirmed_tool_catalog_revision`. Manager genérico no se modificó por este cambio. Un Tool snapshot genera authoring references y dependency evidence.

## Corte B1d histórico — implementación y qualification (2026-09-29)

```text
atlanticus B1d implementado             a518ff98c6303220e24ae3c645d3982e657fd22e
atlanticus inspeccionado posteriormente caced5d7711cf059d36ec61aecc9b3e9629bd41f
atlanticus-decisions leído              50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
canonical leído en corte B1d            ec16bd2ccf0ae06065b8ee1d3a231ef4d2cbac57
canonical HEAD posterior B1d            a5bb42157ee7a5dd2fd64ccc43fa4519628ce25c
```

En este corte HISTÓRICO `backend/tools/discovery-cosmos` y `backend/tools/catalog` eran owners; la UI aún estaba en el host temporal. Esa descripción **quedó SUPERSEDED por C1**; no usarla como arquitectura vigente.

Qualification B1d por logs del usuario: 38 tests backend, 31 Manager Web y 6 qualification, Ruff/format PASS; dos Sources/Projections Tool en Cosmos controlado, reconciliación, confirmación y lectura cruzada en Azurite con revisión `6a26feedc3cf7cee4ebcf5a93ad59314180635875ab25423bb576a052e517243`. Alarm Source `local` release `1d76643e80f849cc931702689aec45a6`; `verify-alarm` durable observó ausencia de Source/Projection. No convertir estos datos en preloading, Azure productivo o requisitos estructurales del producto. El directorio temporal B1d fue eliminado en un incremento posterior; el histórico conserva que estuvo presente localmente.

## Nuevo corte C1 — Tool services pertenecen a Web (2026-09-29)

```text
atlanticus:main verificado en Git           3961385aecd0eb7e373018fc25e509a71dccc409
commit C1                                 3961385aecd0eb7e373018fc25e509a71dccc409
commit previo: tests Runtime              a4dc45fc7fa17ef20e6ddfa828bbb3a471c17f2d
commit intermedio ajeno a C1              d4239806c01f0f8bf4b4d3e680715ca460667bcb
atlanticus-decisions:main                50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
atlanticus-cannonical:main base sustituida faec587c3fb321e76c1a3da38a4d8193a2f1fdb5
```

**VERIFIED remoto:** `web/tools/catalog`, `web/tools/discovery-cosmos` y `web/tools/catalog-manager` son módulos independientes. El Configuration Manager consume la capability UI y compone los servicios, sin código UI duplicado. El árbol Git no contiene archivos versionados bajo `backend/tools`; `domain/tools` mantiene manifest compartido. El commit C1 trasladó dos distribuciones, sus espejos y tests, corrigió importaciones y dependencias y retiró el `uv.lock` redundante de `backend/processes/alarms-materialization`; `backend/uv.lock` es el lock del workspace.

**VERIFIED por logs locales del usuario:** pruebas, Ruff, espejos, `uv lock --check`, generación de los dos wheels `web_tool_catalog` y `web_tool_discovery_cosmos`, importación desde venv de Configuration Manager, verificación de ausencia de package identifiers anteriores y `git diff --cached --check` sin errores. La limpieza física local de `.venv` abandonados bajo `backend/tools` fue indicada; sólo la ausencia de archivos Git versionados está confirmada remotamente.

**UNVERIFIED:** instalación aislada de wheels, navegador/aceptación visual post-C1, CI completa, Azure, Starter y recorrido Alarm durable E2E. No recalificar B2c.7 por esta migración.

## Historical conflicts no corregidos por C1

`atlanticus-decisions` conserva historia posiblemente desactualizada para SharePoint como autoridad física, la re-resolución Rn contra una Tool revision posterior y la salida consolidada Tool a Command Center Cosmos. El Markdown de reglas globales de Manager revisado en este cierre no se opone al ownership Web C1; no se hizo nueva auditoría exhaustiva de los DOCX de decisions. Otros conflictos de alarmas permanecen registrados en sus documentos propios.
