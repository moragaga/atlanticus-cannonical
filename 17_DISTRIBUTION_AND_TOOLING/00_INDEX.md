# Distribution and Tooling — Canonical Index

Estado: **CURRENT — WEB DISTRIBUTION CLEANUP CLOSED AT 2dc5862f**

| Archivo | Alcance | Estado |
|---|---|---|
| `01_BACKEND_GENERATION.md` | Artifacts backend | CURRENT / OTHER FOCUS |
| `02_FRONTEND_GENERATION.md` | Shared Web engine + product-owned starters | CURRENT |
| `03_ARTIFACT_DISTRIBUTION.md` | Generic/ADA/Command Center qualification contracts | CURRENT |
| `04_SCRIPTS_VALIDATION.md` | Validation gates | CURRENT |
| `05_SUPPORT_SERVICES.md` | Cosmos/Storage support | CURRENT DIRECTION |
| `06_ENV_DETAIL.md` | Configuration documentation contract | **PLANNED / NEXT AUDIT** |
| `07_READMES.md` | README policy | CURRENT |
| `08_LOADERS.md` | Loader contracts | CURRENT / OPEN BY FOCUS |
| `09_SOURCE_LEDGER.md` | Distribution/tooling evidence ledger | CURRENT |

## CURRENT layout

```text
tooling/distribution/web/
    shared engine
    products.toml
    starter/base

scopes/ada/tooling/distribution/web/
    ADA-specific support/starter/project tooling

scopes/ada-command-center/tooling/distribution/web/
    Command Center starter
```

## Qualification CURRENT

```text
generic         PASS
ada             PRECHECK_PASS
command-center  PRECHECK_PASS
```

## NEXT

No más refactor de tooling sin finding real.

Siguiente foco:

```text
ADA + Command Center .env.detail contract
```
