# Atlanticus — Validation Baseline

Estado: **CURRENT — USERS TOOL RUNTIME CUTOVER QUALIFIED 2026-10-03**

## Autoridad

```text
Implementation  moragaga/atlanticus@2f9b65c3ba2646d519abfb0bb49e095d6819d185
Decisions       moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

## Evidencia focal reportada por el usuario

```text
Users Core                    44 passed
Users Blob                     7 passed
Users Cosmos                   7 passed
Master Projection             53 passed
ADA Configuration Manager     65 passed
ADA Generic Application      199 passed
```

## Acredita

```text
Global UserIdentity + ToolUserMembership
RuntimeUser
Tool-owned users-runtime without app/tool fields
Blob identity/membership separation
Tool recovery snapshot + REPLACE
Master Projection Users
Manager/session integration
Jane/John palettes
Operational membership consumer
```

## No acredita

```text
monorepo-wide pytest
CI
new Docker artifact after current HEAD
new isolated consumer after current HEAD
Azure/Entra
Navigation PUBLIC/RESTRICTED
production-like destructive recovery
multiworker revocation
```

Un `git diff --check` del worktree del usuario mostró errores EOF en archivos KPI ajenos a este incremento. No atribuirlos a Users.
