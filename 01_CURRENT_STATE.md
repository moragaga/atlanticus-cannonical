# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT — WEB DISTRIBUTION TOOLING CLEANUP CLOSED**

## Autoridad

```text
Implementation
moragaga/atlanticus@2dc5862f634eb0bf8fe72d771d56605d1c7f32cf

Historical decisions
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e

Canonical inspected before replacement
moragaga/atlanticus-cannonical@852e031d020edd4fdd5ab0187e95fbf6f443083b
```

Git permanece **SOLO LECTURA**.

## Estado resumido

```text
WEB-DISTRIBUTION-SHARED-ENGINE-CLEANUP          CLOSED / VERIFIED / CURRENT
ADA-STARTER-RUNTIME-THINNING                    CLOSED / VERIFIED / CURRENT
ADA-MASTER-PROJECTION-RUNTIME-OWNERSHIP         CLOSED / VERIFIED / CURRENT
COMMAND-CENTER-DISTRIBUTION-PROFILE             CLOSED / VERIFIED / CURRENT
CROSS-PLATFORM-WHEELHOUSE-SDIST-FALLBACK        CLOSED / VERIFIED / CURRENT

GENERIC-WEB-DISTRIBUTION                        PASS / VERIFIED
ADA-WEB-DISTRIBUTION                            PRECHECK_PASS / VERIFIED
COMMAND-CENTER-WEB-DISTRIBUTION                 PRECHECK_PASS / VERIFIED

ADA-AND-COMMAND-CENTER-ENV-DETAIL-CONTRACT      PLANNED / NEXT
DUAL-APP-STORAGE-FINAL-COSMOS-LOCAL-SMOKE       PLANNED / AFTER ENV CONTRACT
ADA-KPI-COLLECTOR-OPERATIONAL-E2E               PLANNED / AFTER DUAL-APP SMOKE
ADA-UI-RECONSTRUCTION                           PLANNED / AFTER COLLECTOR
```

## Shared Web distribution — CURRENT

```text
tooling/distribution/web/
├── build_wheelhouse.py
├── distribute.py
├── generate_starter.py
├── probe_starter.py
├── qualify_starter.py
├── products.toml
└── starter/base/
```

No contiene subárbol `ada/`, starter ADA ni starter Command Center.

Product-specific ownership:

```text
scopes/ada/tooling/distribution/web/
scopes/ada-command-center/tooling/distribution/web/
```

## ADA Generic — CURRENT

`ada-generic-application==0.2.21` posee:

```text
runtime host/lifecycle
local/durable persistence composition
Master Projection runtime
Master material reader/provisioning
local resource preparation
```

El Starter ADA no posee una segunda implementación de esas responsabilidades.

Master Projection tiene identidad derivada:

```text
master-projection/material.zip
```

En local se resuelve bajo el namespace de aplicación.
En durable se resuelve como Blob bajo el namespace de aplicación.

## Command Center — CURRENT

`ada-command-center-generic-application==0.1.0` existe como composition root real y dispone de distribución propia.

Runtime CURRENT:

```text
local-only Manager host
Storage real requerido por Tool Catalog incluso en local
production identity/durable host no implementados
Command Center Master Projection no integrado
```

Su `.env.detail` todavía refleja el contrato local actual; no declarar durable Command Center como implementado.

## Qualification de este cierre

Evidencia local reportada por el usuario:

```text
tooling/tests/distribution/web                     125 passed, 3 skipped
compose integration subset                         10 passed, 3 skipped
Ruff / format                                      PASS
git diff --check                                   PASS

generic distribution
  starter                                          PASS
  wheelhouse packages                              36
  qualification                                    PORTABLE / PASS
  readiness                                        ready

ada distribution
  starter                                          PASS
  internal wheels                                  71
  qualification                                    PRECHECK_PASS
  runtime/image                                    UNVERIFIED

command-center distribution
  starter                                          PASS
  wheelhouse packages                              85
  dependency_check                                 PASS
  qualification                                    PORTABLE / PRECHECK_PASS
  runtime                                          UNVERIFIED
```

## Cross-platform wheelhouse — CURRENT

Registry dependency selection:

```text
compatible SHA256-locked wheel
→ preferred

no compatible wheel
+ SHA256-locked sdist
→ build platform wheel locally
→ hash-constrained PEP 517 build dependencies
→ distribute wheel only
```

El manifest registra el hash del sdist de origen y del wheel construido.

Esta corrección fue validada en macOS con Generic y Command Center.

## ADA UI / no-data observation

CURRENT verificado:

```text
ContentState:
READY
STALE
SOURCE_ERROR
CONSTRUCTION

ContentStatePresentationMode:
NORMAL
AUTHORING
```

`AUTHORING` suprime visualmente el overlay degradado, pero no altera el estado real.

No existe un estado explícito `NO_DATA` / `WAITING`.

Por tanto:

- diseño visual sin watermark es posible mediante el contrato authoring;
- que todos los componentes puedan renderizar sin datos reales sigue **UNVERIFIED**;
- no clasificar automáticamente ausencia inicial de datos como error sin revisar el contrato Collector/UI.

## Próxima frontera

```text
ADA-AND-COMMAND-CENTER-ENV-DETAIL-CONTRACT
PLANNED / NEXT
```
