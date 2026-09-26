# ADA Generic — Source Ledger

Estado: **AUDIT LEDGER / NAVIGATION LOCAL CLOSURE 2026-09-25**

## Autoridades observadas para el delta

```text
IMPLEMENTATION (READ ONLY)
moragaga/atlanticus@a6061ffed59c8b04e64b0a7fdc17050ef463c850
parent: b36c7ee6ace89f18af25602f9e3a546a16e7d6f0
tree: 2d81323806bbde09d1b71f052bf3000b3399ec0d

CANONICAL BEFORE REPLACEMENT
moragaga/atlanticus-cannonical@55c531b192fedcc6343b3c9e2ee1f9ec4ffa8fab

HISTORICAL DECISIONS
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

Git remoto no fue modificado durante el cierre.

## Checkpoints previos conservados

```text
21cfb2f11362c1606ad14ff8adc7551948eced6a  Tool persistence composition
01a4387d9f73aceb83441d2f26f94ad9025a661c  Operational bootstrap
940336d5b704d10280cc2375e68c60b45f235eb0  Pydantic dependency hygiene
d6e405e6466b1bf8d29dadae442a03062da2f1b3  Collector runtime wiring
bc8eafc21a65e3f9aff044c232e2562cd490c49f  Render structural cutover
```

Qualification histórica: bootstrap 83 tests; collector wiring 87; cutover de Render
149 tests agregados en las capacidades implicadas. Ver detalles y límites en checkpoints
anteriores: no equivalen a rerun final de este cierre.

## Navigation de este hito

```text
9c9cc19810e9ab72a95e1973886d91e9ea9182c0  Navigation Core recovery baseline
9abf54a765137b1250476aa7b5b4737cf6f79a35  HEAD previo a integración
3418872a615463970b8738d1ec25f16cb2fe1fff  Integración Navigation + reparaciones
b36c7ee6ace89f18af25602f9e3a546a16e7d6f0  E2E test agregado
 a6061ffed59c8b04e64b0a7fdc17050ef463c850  Correctivo actual callback/controller
```

`9c9cc1`: Navigation Core informó `29 passed` y Ruff verde en un hito previo.
`3418872`: antes del E2E, el usuario informó `169 passed`/Ruff verde en ADA Generic y
`39 passed`/Ruff verde en archivos seleccionados de Configuration Manager.
Durante el correctivo cliente (antes de `a6061ffe`), el usuario informó ADA Generic
`172 passed`/Ruff verde, shell `8 passed, 1 skipped` y un error Ruff de import order en
el test nuevo. No hay rerun aportado de todos los tests sobre `a6061ffe`.

**VERIFIED MANUAL declarado:** usuario persistió/publicó/proyectó Navigation local,
consumió menú desde Home y confirmó funcionamiento del menú tras la última corrección.

**VERIFIED STATIC:** `a6061ffe` contiene controller fuera del Offcanvas, Store de ruta,
tratamiento explícito del callback clientside y eliminación del `title` nativo.

**UNVERIFIED:** qualification persistente Blob/Cosmos tras reinicio, browser matrix,
Node y Ruff de todos los archivos en HEAD, Entra e imagen Web distribuidora.

## Contratos eliminados / no restaurar

`_StartupToolProjectionStore`, `resolve_current_tool_projection`, binding Collector→Render
con KPI snapshot y menú operacional fijo como autoridad no son contratos vigentes.
No reintroducir adaptadores legacy.

## Conflict ledger

- Canonical `15_WEB_PLATFORM/12...` declaraba `enabled=False → deny` sin excepción y
  `NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT BLOCKED`. Código actual implementa
  `administrative_override` excepcional; el documento se reemplaza en este paquete.
- Canonical `17_DISTRIBUTION_AND_TOOLING/03...` cita `scripts/local-process.sh` como existente.
  Ese path no aparece en el árbol del commit final inspeccionado. No sustituirlo por supuesto
  tooling; auditar el flujo real de distribución.
- Python objetivo Project `3.14.7`, mientras `deployment/processes/bundle.py`, Dockerfile
  y metadata de paquetes inspeccionados aún exigen `3.14.2`.
- `00_INDEX.md` y `01_CURRENT_STATE.md` globales tienen checkpoints previos. Su consolidación
  es transversal y no se sobreescribe desde este delta limitado para evitar pisar otros frentes.

## Estado

`ADA-GENERIC-NAVIGATION-LOCAL`: **CLOSED / VERIFIED MANUAL / CURRENT**.
`ADA-GENERIC-REAL-DISTRIBUTION`: **PLANNED / UNVERIFIED**.
