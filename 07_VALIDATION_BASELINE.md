# Atlanticus — Validation Baseline

Estado: **CURRENT — WEB DISTRIBUTION TOOLING CLEANUP / CROSS-PLATFORM QUALIFICATION 2026-10-02**

## Autoridad

```text
Implementation
moragaga/atlanticus@2dc5862f634eb0bf8fe72d771d56605d1c7f32cf

Decisions
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

La evidencia corresponde a ejecuciones locales reportadas por el usuario. No equivale a CI, Azure o infraestructura productiva.

## Tooling Web — VERIFIED

```text
tooling/tests/distribution/web
125 passed, 3 skipped

compose integration focused gate
10 passed, 3 skipped

Ruff check
PASS

Ruff format --check
PASS

git diff --check
PASS
```

Los 3 skips corresponden a checks de Docker Compose cuando el runtime Docker no está disponible; no tratarlos como PASS de Docker.

## Generic Web distribution — VERIFIED

```text
starter               PASS
wheelhouse packages   36
qualification         PORTABLE / PASS
health.live            PASS
health.ready           ready
home.http              PASS
dash.layout            PASS
```

El runtime portable fue ejecutado durante qualification.

## ADA Web distribution — VERIFIED PRECHECK

```text
starter               PASS
internal wheels       71
distribution          BUILT_UNQUALIFIED
qualification         PRECHECK_PASS
image_build           UNVERIFIED
runtime               UNVERIFIED
```

No declarar Docker/runtime ADA verificado por este resultado.

## Command Center Web distribution — VERIFIED PRECHECK

```text
starter               PASS
wheelhouse packages   85
dependency_check      PASS
qualification         PORTABLE / PRECHECK_PASS
runtime               UNVERIFIED
```

No declarar Storage runtime/Command Center startup verificado por este artifact precheck.

## macOS wheelhouse portability — VERIFIED

Generic y Command Center inicialmente bloquearon por:

```text
rcssmin==1.2.2
No SHA256-locked compatible wheel
```

El builder compartido fue corregido para usar un sdist SHA256-locked cuando no existe wheel compatible y construir un wheel de plataforma con build dependencies hash-constrained.

Después del cambio:

```text
test_build_wheelhouse.py    12 passed
tooling suite               125 passed, 3 skipped
generic distribution        PASS
command-center              PRECHECK_PASS
```

## Límites

UNVERIFIED en este cierre:

```text
ADA Docker image/runtime
Command Center runtime con Storage
Command Center durable Manager
Command Center Master Projection
Azure/Entra
production secrets/Key Vault
dual application Storage-final + Cosmos-local smoke
ADA KPI Collector against real delivery data
UI rendering with no data across all components
```
