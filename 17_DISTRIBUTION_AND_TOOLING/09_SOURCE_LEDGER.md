# Distribution and Tooling — Source Ledger

Estado: **AUDIT LEDGER — DYNAMIC DEPLOYMENT RESOURCES CURRENT**

## Authorities

```text
Implementation  moragaga/atlanticus@5c40faed4df7f3d7b6db79251144a9ec09e09e91
Decisions       moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical base  moragaga/atlanticus-cannonical@15a51f70396726a2ad3b88d1afc66ce8cfff3300
```

## Current implementation evidence

Checkpoint `5c40faed4df7f3d7b6db79251144a9ec09e09e91` contains:

```text
tooling/distribution/processes/consumer/deployment_resources.py
tooling/distribution/processes/consumer/AZURE_CONTAINER_APPS_RESOURCES.md
tooling/distribution/processes/distribute.py resource generation/preservation
tooling/distribution/processes/consumer/process.py dynamic resource execution/integration
deployment/local/simulation.py explicit resource inputs
deployment/local/generate_compose.py runtime-input alignment
tooling/gates/process-deployment/check.py aligned Docker contract
focused tests for deployment resources
```

## Qualification evidence

User-reported local gate:

```text
31 deployment/process tests
16 local deployment tests
8 local process tooling tests
56 distribution process tooling tests
111 total
Ruff/format PASS
shell launcher syntax PASS
```

## Decisions refined during implementation

```text
pyproject resource authority                SUPERSEDED
persistent Compose resource authority       SUPERSEDED
update-deployment synchronization command   SUPERSEDED
dynamic consumer resource file              CURRENT
filtered full process-root Docker COPY      CURRENT
secrets/connections runtime inputs          CURRENT
```

## Open evidence gap

Extension integration has implementation for resource initialization but lacks focused proof of:

```text
preserve existing custom sizing
default new component sizing
rollback after resource-file publication
legacy-distribution behavior
```

## Next ordering

```text
extension resource integration qualification
→ return to full artifact regeneration/qualification
→ .env.detail audit
→ final distribution consumer qualification
```
