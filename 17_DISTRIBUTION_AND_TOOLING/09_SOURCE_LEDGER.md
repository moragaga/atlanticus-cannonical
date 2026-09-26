# Distribution and Tooling — Source Ledger

Estado: **AUDIT LEDGER / 2026-09-25 DELTA**

```text
Implementation observed
moragaga/atlanticus@a6061ffed59c8b04e64b0a7fdc17050ef463c850

Canonical before this replacement
moragaga/atlanticus-cannonical@55c531b192fedcc6343b3c9e2ee1f9ec4ffa8fab

Historical decisions
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

## Existencia inspeccionada en CURRENT

```text
deployment/processes/bundle.py
 deployment/processes/Dockerfile
scripts/scopes/ada/backend/check.py
scripts/backend/check.py
scripts/web/check.py
scopes/ada/backend/processes/kpi-{runtime,delivery,historian,timeseries-delivery}
scopes/ada/web/application/ada-generic-application/pyproject.toml
scopes/ada/web/application/ada-generic-application/.env.detail
```

La agrupación `kpi-{...}` arriba es notación documental, no un path de runtime.

## Desfase documental detectado

El ledger previo citaba `scripts/local-process.sh` como existente. El árbol git
inspeccionado para `a6061ffe` no lo contiene: `UNVERIFIED/STALE REFERENCE`.
No recomendar sus comandos hasta encontrar un reemplazo real y verificado.

La metadata de Python inspeccionada exige `3.14.2`, frente al objetivo Project `3.14.7`.
Queda `CONFLICT` de distribución, sin cambio de implementación autorizado aquí.

## Criterio de evidencia

`uv run` y persistencia/proyección Navigation local validados manualmente no equivalen
a bundle de proceso construido ni Web portable. Build, instalación limpia,
recovery, Docker real/emulado, Entra y distribución externa permanecen separados.

El desarrollo backend y la distribución Web comparten el principio artifact/distribution,
pero no se acoplan en un único generador.
