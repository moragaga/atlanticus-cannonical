# ADA Web — Current Baseline

Estado: **CANDIDATE**

## Implementación actual

`scopes/ada/web/` contiene fronteras dedicadas para, entre otras:

- alarms;
- application;
- branding;
- components;
- configuration;
- content-state;
- inspection;
- kpis;
- operational-render-binding;
- operational-state;
- runtime-experience;
- shell.

`ada-generic-application` actual está en versión `1.0.6` en el commit auditado.

Consume capacidades versionadas como:

- `ada-web-alarms-management==0.1.6`;
- `ada-web-alarms-status==0.1.6`;
- `ada-web-branding==0.2.0`;
- `ada-web-content-state==0.1.0`;
- `ada-web-operational-render-binding==0.1.1`;
- `ada-web-operational-state==0.1.0`;
- `ada-web-runtime-experience==0.1.0`;
- `ada-web-shell==0.4.2`;
- `ada-web-ui-nav==0.4.1`;
- `ada-web-ui-time-status==0.1.15`;
- `atlanticus-web==0.8.4`.

## Python

El paquete actual aún declara Python `>=3.14.2,<3.15`.

La baseline objetivo del Project es Python 3.14.7/Trixie.

Clasificación:

`DECIDED / NOT YET IMPLEMENTED`

## Checkpoint histórico

El checkpoint del 31-08-2026 registró como GREEN, entre otros:

- SESSION-AUTO-000-001..005;
- WAKE-PULSE-001;
- PWA-SURFACE-001;
- PAGE-READY-001;
- WAKE-LOCK-001;
- ACTIVITY-001;
- CARD-DISPLAY-001;
- RESPONSIVE-HEADER-001.

En ese documento:

`RESPONSIVE-TIME-001`

seguía como candidato, aunque la capability `ada-web-ui-time-status==0.1.15` sí está físicamente presente en `main` actual.

No declarar `RESPONSIVE-TIME-001` GREEN sin evidencia de closure posterior.

## Regla

No reabrir incrementos históricos GREEN sin finding real.

Pero un checkpoint histórico no sustituye verificación contra `main` para conocer versiones y composición vigentes.
