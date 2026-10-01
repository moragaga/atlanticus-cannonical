# ADA Command Center — Source Ledger

Estado: **AUDIT LEDGER — preserve B1d/B2c.7/C1/C2/C4 history; append 2026-10-01 Command Center administration and Generic Application composition checkpoints. Historical SHAs are not redefined as current HEAD.**

## Authority of this replacement

```text
Implementation CURRENT
moragaga/atlanticus:main@736ae9820878a5d8ec7fa7f922ce483be3d3e6b3

Administration composition
moragaga/atlanticus@2ccc2dffd792d55ae68aee3d64ed73eef408bbf8

Generic Application
moragaga/atlanticus@736ae9820878a5d8ec7fa7f922ce483be3d3e6b3

Decisions
moragaga/atlanticus-decisions:main@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e

Canonical base before this replacement
moragaga/atlanticus-cannonical:main@226bcded9eb6970292c7722b23b595e93b4a9cff
```

This documentation close does not write Git.

## Previous Domain / Source v3 cut — HISTORICAL

```text
atlanticus old checkpoint      880cb692054c2cd78cdc29cf62ab7b16bbd2c3d6
Domain Tools / Manifest        9b9600ae96c9153cf70d0fb401905963b8583c2f
Alarm Source Snapshot v3       d2a5e14822d3711e64668b8e70cfa15d7ddae2f0
historical canonical           148b178df74ee3083681140f3bb7997a02435b80
```

`domain/tools` introduced `ToolDependencyEntry` and `ToolDependencyManifest`. `AlarmConfigurationSnapshot(configuration, tool_dependencies)` preserves frozen Cn derived from manifest. Source v2 is SUPERSEDED, v3 CURRENT, with no v2 decoder. Workspace keeps `_confirmed_tool_catalog_revision`, Validate/Publish drift guard and exact dependencies. Historical local evidence: Domain Tools 8, Domain Alarms 50, Alarm Web 35, Configuration Manager 11.

## B1d — Tool local qualification HISTORICAL

```text
Implementation B1d        a518ff98c6303220e24ae3c645d3982e657fd22e
Later inspection          caced5d7711cf059d36ec61aecc9b3e9629bd41f
Decisions                 50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical at cut          ec16bd2ccf0ae06065b8ee1d3a231ef4d2cbac57
Later canonical B1d       a5bb42157ee7a5dd2fd64ccc43fa4519628ce25c
```

B1d locally qualified two controlled Tool Sources/Projections Cosmos, discovery/confirmation and Blob CURRENT in Azurite. Observed Tool Catalog revision: `6a26feedc3cf7cee4ebcf5a93ad59314180635875ab25423bb576a052e517243`. Reported evidence was 38 backend tests, 31 Manager and 6 qualification plus Ruff/format PASS. Alarm Source local did not certify Alarm Source Blob/Projection Cosmos durable. Temporary qualification files were cleaned. Historical `backend/tools` ownership was SUPERSEDED in C1.

## C1 — Tool Web ownership CLOSED

```text
atlanticus C1            3961385aecd0eb7e373018fc25e509a71dccc409
previous commit          a4dc45fc7fa17ef20e6ddfa828bbb3a471c17f2d
intermediate commit      d4239806c01f0f8bf4b4d3e680715ca460667bcb
canonical base C1        faec587c3fb321e76c1a3da38a4d8193a2f1fdb5
```

`web/tools/catalog`, `web/tools/discovery-cosmos` and `web/tools/catalog-manager` became owners of Web services, UI/callbacks and host composition. `backend/tools` ceased to be the versioned owner without compatibility adapters. Isolated distribution, final browser, CI, Azure and Starter remained UNVERIFIED.

## B2c.7 — independent operational history

At `atlanticus@c67fcb5b105cc561c16719a8bca4ea5aa74c3fae`, Runtime already produced CURRENT v1 and durable FACTS v2. The receiver at that time consumed both. B2c.7 remains HISTORICAL and was SUPERSEDED by C4 only for Delivery receiver behavior. FACTS v2 production by Runtime was not removed.

## C2 — identity, volume and topology (HISTORICAL / CURRENT CONTRACT)

```text
Base before C2           53c20b2462e8637bd00f75487b0df05dc8452daa
Final C2 HEAD            18029e19ff01e58b9c9399c132ff32b5ca913f06
Decisions C2             50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical C2 integrated  2e8bbf4780cafc4cea3b18351861aa97a4fb0053
```

**DECIDED C2:** the three jobs use `APPLICATION=ada-command-center` while retaining independent `job_key`/leases. `VOLUMEN_PATH` is absolute and operator-managed. Source key `alarm-configuration` belongs to Domain. Cosmos physical name/partition derive from the resource contract, not duplicated environment variables. Blob container remains environment-supplied.

**VERIFIED Git C2:** Domain exports the constant; Web constructs `SourceKey`; all three processes derive `settings.source_key`; Materialization consumes the resource contract and removed `ALARM_PROJECTION_CONTAINER`. Physical multi-host equivalence and Docker remained OPEN.

## C4 — Delivery receiver CURRENT-only (2026-09-29)

```text
Base C4       18029e19ff01e58b9c9399c132ff32b5ca913f06
HEAD C4       45eff96d777f4711cb011f779ffc0a6c87bf0ca4
Decisions     50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical     2e8bbf4780cafc4cea3b18351861aa97a4fb0053
```

**DECIDED:** Delivery consumes only latest CURRENT, waits when CURRENT/EFFECTIVE/READY do not exactly align and retries in a later cycle. No extra synchronization is introduced. Runtime retains FACTS v2/WAL independently.

**VERIFIED Git:** the C4 delta was isolated under `backend/processes/alarms-delivery`; receiver removed FACTS cursor/consumption and consumes latest CURRENT with integrity, EFFECTIVE/pin/READY, UTC time and fence checks. Runtime files were not modified.

**VERIFIED by local logs:** 29 PASS Delivery, 16 PASS Runtime publishers, Ruff PASS, `git diff --check` and `uv lock --check` PASS; global regression later reported 567 PASS / 1 SKIPPED after restoring the full workspace environment.

**UNVERIFIED:** separate Docker, CI, Azure, shared physical mount, production scheduler timing and Live delivery.

## Manager convergence — 2026-10-01 HISTORICAL CHECKPOINT

```text
HEAD
0831cb8b85c28b71afeaea42207771ca8527f0d3

atlanticus-web-manager
0.3.19

Command Center aligned packages
ada-command-center-web-alarm-configuration 0.1.1
ada-command-center-web-tool-catalog-manager 0.1.1
ada-command-center-configuration-manager 0.1.1
```

Alarm Configuration delegated generic workspace mechanics to `ManagerWorkspaceBinding`, retaining only Tool Catalog confirmation/revision pinning as domain specialization.

This checkpoint did not yet include Users/Profiles/Navigation or the real product Generic Application.

## Command Center administration integration — 2026-10-01 CLOSED

```text
Base
0831cb8b85c28b71afeaea42207771ca8527f0d3

HEAD
2ccc2dffd792d55ae68aee3d64ed73eef408bbf8

Package
ada-command-center-configuration-manager 0.1.2
```

**VERIFIED Git:** the configuration-manager package now composes generic Atlanticus Users, Profiles and Navigation management alongside existing Tool Catalog and Alarm Configuration. `NAVIGATION_SOURCE_KEY` is `navigation`. `CommandCenterAdministrationDependencies` exposes Profiles/Navigation projections for product consumers.

**VERIFIED local by user logs:**

```text
30 PASS
Ruff check PASS
Ruff format check PASS
git diff --check PASS
```

Local administration projections and Users state are in-process; durable topology was not established by this cut.

## Command Center Generic Application — 2026-10-01 CLOSED / CURRENT

```text
Base
2ccc2dffd792d55ae68aee3d64ed73eef408bbf8

HEAD
736ae9820878a5d8ec7fa7f922ce483be3d3e6b3

Package
ada-command-center-generic-application 0.1.0
```

**VERIFIED Git:** new real product composition root exists under `scopes/ada-command-center/web/application/ada-command-center-generic-application`. It exposes Home `/`, Manager `/manager`, Identity, projected operational Navigation and reuses the configuration-manager composition. It does not import ADA Access, ADA KPI Registry/Definition, ADA Collector, ADA Tool operational projection or ADA-specific shell.

**VERIFIED local by user logs:**

```text
6 PASS
Ruff check PASS
Ruff format check PASS
git diff --check PASS
local application smoke PASS reported by user
```

The local launcher uses `LocalIdentityProvider` and local administrative `ManagerPrincipal`; production fails fast until a production identity provider/runtime is injected.

The current Manager header remains generic because Command Center does not configure product-specific brand marks/title/subtitle. This is accepted and deferred.

## Current sequencing checkpoint

The previous canonical sequence that blocked tooling until Generic existed is now satisfied.

Current next boundary:

```text
ADA-COMMAND-CENTER-DUAL-PRODUCT-TOOLING-DISTRIBUTION
PLANNED / NEXT
```

Resource Preparation/startup gate remains PLANNED / DEFERRED.

## Conflicts and unresolved authority

1. Canonical base `226bcd...` still described Generic Application as not implemented and tooling as blocked. That is SUPERSEDED by implementation HEAD `736ae...`.
2. `11_GOLDEN_PATH.md` named Resource Preparation as the unique next focus. That sequencing is SUPERSEDED by the later product-root close and explicit next-focus decision; Resource Preparation itself remains open.
3. Decisions `50c2...` contains no located explicit decision for the new Generic Application or dual-product tooling sequence. Do not invent one; the current sequence comes from implemented reality, canonical prior sequencing and explicit Project decisions.
4. Current Command Center packages still reuse some code under `scopes/ada` (`AdaStorageNamespace`, `ada-web-tools`). This is existing code and a product-boundary concern to audit during tooling; do not silently rewrite it.
5. Current relevant Web packages still declare Python `==3.14.2`, while the Project target baseline is 3.14.7/Trixie. Migration remains separate and deferred.

## Historical conflicts preserved

Older B.1/B.2 Decisions documents contain historical SharePoint/reconciliation/output formulations. Current implementation/canonical contracts establish Blob for Tool Catalog, durable Alarm Source objective with Cosmos Projection input, READY local and Runtime WAL/EFFECTIVE. Live and Management remain separate. No exhaustive rewrite of historical Decisions is implied by this close.
