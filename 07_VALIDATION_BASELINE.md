# Atlanticus — Validation Baseline

Estado: **CURRENT — DUAL APP DURABLE COMPOSITION + MASTER PROJECTION CONVERGENCE 2026-10-02**

## Autoridad

```text
Implementation evidence checkpoint
moragaga/atlanticus@7bd11afdf2af82c56fb100f4aa5336c039d9bd22

Decisions
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

La evidencia siguiente es local/user-reported. No equivale a CI, Docker actual, Azure ni producción.

## Durable runtime convergence — VERIFIED

Command Center Configuration Manager:

```text
uv lock     PASS
pytest      31 passed
Ruff        PASS
format      PASS
```

Command Center Generic después de Master Projection:

```text
pytest      10 passed
Ruff        PASS
format      PASS
```

ADA Generic después de extracción Master Projection:

```text
pytest      249 passed
Ruff        PASS
format      PASS
commented host AST mirror PASS
```

Shared Master Projection:

```text
atlanticus-web-master-projection==0.1.0
pytest      56 passed
Ruff        PASS
format      PASS
```

Repository:

```text
git diff --check PASS
```

## Qué acredita

VERIFIED:

```text
shared Master Projection engine compiles/tests independently
ADA consumes shared engine without duplicate engine files
Command Center consumes shared engine
Command Center local host can select durable Manager composition
.env.detail contracts were aligned with environment/persistence separation
```

## Qué NO acredita

UNVERIFIED:

```text
real Storage/Cosmos connectivity for both applications in this hito
Command Center resource preparation automation/parity
Master material generation + login in both current product builds
Docker/image runtime from current packages
regenerated current-head Web distribution artifacts
Azure/Entra
production Key Vault/secrets
KPI/Collector/UI E2E
```

## Historical distribution evidence

Earlier artifacts reached:

```text
generic         PASS
ADA             PRECHECK_PASS
Command Center  PRECHECK_PASS
```

Those artifacts precede the current package/version changes.

Do not reuse those statuses as current-head artifact qualification without regeneration.
