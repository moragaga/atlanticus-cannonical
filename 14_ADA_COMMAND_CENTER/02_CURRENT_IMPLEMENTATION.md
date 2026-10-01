# ADA Command Center — Current Implementation

Estado: **CURRENT — Source v3/Materialization/Runtime, C1 ownership Web, C2 identity and C4 Delivery CURRENT-only preserved; Manager administration composition CURRENT; Command Center Generic Application 0.1.0 CURRENT / VERIFIED LOCAL; dual-product tooling/distribution NEXT.**

## Authority checkpoints

```text
Implementation CURRENT
moragaga/atlanticus:main@736ae9820878a5d8ec7fa7f922ce483be3d3e6b3

Administration composition close
moragaga/atlanticus@2ccc2dffd792d55ae68aee3d64ed73eef408bbf8

Generic Application close
moragaga/atlanticus@736ae9820878a5d8ec7fa7f922ce483be3d3e6b3

Decisions
moragaga/atlanticus-decisions:main@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e

Canonical base before this replacement
moragaga/atlanticus-cannonical:main@226bcded9eb6970292c7722b23b595e93b4a9cff
```

Historical checkpoints for C1/C2/C4 retain their own evidence and are not rewritten by this close.

## Componentes existentes

```text
scopes/ada-command-center/
  domain/alarms/
  domain/tools/
  backend/alarms/core/
  backend/alarms/materialization/
  backend/alarms/persistence/
  backend/alarms/contracts/
  backend/processes/alarms-materialization/
  backend/processes/alarms-runtime/
  backend/processes/alarms-delivery/
  web/alarms/configuration/
  web/alarms/persistence/
  web/alarms/projection-local/
  web/alarms/projection-cosmos/
  web/tools/catalog/
  web/tools/discovery-cosmos/
  web/tools/catalog-manager/
  web/application/ada-command-center-configuration-manager/
  web/application/ada-command-center-generic-application/
```

`backend/tools` está SUPERSEDED. Domain Tools conserva contrato transversal; los servicios usados exclusivamente por Web viven en Web.

Materialization sigue importando `web/alarms/projection-cosmos`: frontera técnica OPEN heredada y no modificada en este hito.

## Manager and administration — CURRENT

Single Manager authority:

```text
atlanticus-web-manager==0.3.19
```

Command Center aligned packages:

```text
ada-command-center-web-alarm-configuration==0.1.1
ada-command-center-web-tool-catalog-manager==0.1.1
ada-command-center-configuration-manager==0.1.2
```

`ada-command-center-configuration-manager==0.1.2` compone además capacidades administrativas genéricas Atlanticus:

```text
Users Manager
Profiles Manager
Navigation Manager
Tool Catalog Manager
Alarm Configuration Manager
```

Contratos relevantes:

```text
USERS_MANAGER_ACCESS_KEY      = users.manage
PROFILES_MANAGER_ACCESS_KEY   = profiles.manage
NAVIGATION_MANAGER_ACCESS_KEY = navigation.manage
NAVIGATION_SOURCE_KEY         = navigation
```

Profiles y Navigation usan Source genérico y ProjectionStore. En el runtime local actual las proyecciones de Profiles y Navigation son in-process y Users administration/registry también es in-process; no declarar persistencia durable ni supervivencia a restart para esos estados.

Qualification local reportada:

```text
30 PASS
Ruff check PASS
Ruff format check PASS
git diff --check PASS
```

## Generic Application — CURRENT

Package:

```text
ada-command-center-generic-application==0.1.0
```

Role:

```text
real Command Center Web product composition root
```

Current implementation:

- application id `ada-command-center-generic-application`;
- display name `ADA Command Center`;
- script `ada-command-center-generic-application`;
- Home page at `/`;
- Manager surface at `/manager`;
- Atlanticus Identity module;
- projected Navigation consuming the same administration navigation projection store;
- Navigation authorization;
- Manager composition reused from `ada-command-center-configuration-manager==0.1.2`;
- local launcher uses `LocalIdentityProvider` and one local administrative `ManagerPrincipal`;
- production startup fails fast because 0.1.0 has no injected production identity/runtime composition yet.

The Generic package depends on the configuration-manager package as a composition provider. It does not execute the standalone configuration-manager application.

Qualification local reportada:

```text
6 PASS
Ruff check PASS
Ruff format check PASS
git diff --check PASS
local smoke PASS reported by user
```

## Application-role boundary — CURRENT

```text
ADA
  ada-generic-application
  ada-configuration-manager

Command Center
  ada-command-center-generic-application
  ada-command-center-configuration-manager
```

The `*-generic-application` is the product composition root.

The `*-configuration-manager` is a separate development/testing/qualification application. It is not a service dependency and is not to be absorbed as a host.

Composition code may be reused where already exposed by the package.

## Explicit product exclusions — CURRENT

Command Center Generic does not incorporate:

```text
ADA Access
ADA KPI Registry / Definition
ADA Collector
ADA Tool operational projection
ADA-specific shell
```

Do not infer those capabilities by symmetry with ADA Generic.

## Alarm Configuration workspace — CURRENT

Alarm-specific responsibility remains:

```text
Save Draft
→ Confirmed Tool Catalog required
→ current catalog revision pinned into workspace payload
```

Generic workspace responsibility remains delegated to:

```text
ManagerWorkspaceBinding
```

No legacy wrapper or compatibility alias was reintroduced.

## Identidad y persistencia de Alarm Configuration — CURRENT

```text
ALARM_CONFIGURATION_SOURCE_KEY = 'alarm-configuration'
```

Physical projection identity:

```text
logical_id       ada.command_center.alarms.configuration.projection
physical_name    alarm-configuration
Cosmos PK        /partition_key
```

Local projection:

```text
<base_root>/conciencia_situacional/command-center/projections/alarm-configuration/
```

Durable Cosmos container:

```text
alarm-configuration
```

Tool Catalog continues to use Storage even under the local Manager provider.

## C2 preserved — processes and volume

`APPLICATION=ada-command-center` identifies Materialization, Runtime and Delivery, which retain independent `job_key` and leases.

`VOLUMEN_PATH` remains manual, absolute and must reference the same physical mount when shared behavior is required.

Operational root remains:

```text
VOLUMEN_PATH/ada-command-center/alarms
```

Physical endpoint/database/credential equality across hosts remains UNVERIFIED.

## Pipeline CURRENT preserved

1. Source v3 freezes `AlarmConfigurationSnapshot(configuration, tool_dependencies)` with exact Rn/Cn references.
2. Materialization consumes ProjectionRecord plus current qualification mechanism and publishes READY or BLOCKED.
3. Runtime adopts an exact pin through WAL/EFFECTIVE.
4. Runtime publishes CURRENT v1 and FACTS v2 on separate channels.
5. Delivery consumes only latest CURRENT and requires exact alignment with EFFECTIVE/READY.

The Web composition increments did not alter these contracts.

## Known implementation boundary conflicts — OPEN

The current Command Center configuration package still imports/reuses some packages under `scopes/ada`, including `ada.web.storage.namespace.AdaStorageNamespace` and `ada-web-tools`. This is existing implemented reality and must not be hidden.

It conflicts with the desired product-isolation direction if those dependencies prove ADA-specific rather than generic. Do not replace them automatically. The next tooling/distribution audit must determine whether they are legitimate shared contracts in the wrong namespace or real cross-product coupling before any remediation is proposed.

## OPEN separados

- **Dual-product tooling/distribution:** PLANNED / NEXT.
- **Command Center Manager product-specific header/branding:** PLANNED / DEFERRED.
- **Production identity / Entra integration for Generic 0.1.0:** PLANNED / UNVERIFIED.
- **Durable Users/Profiles/Navigation topology:** PLANNED / UNFROZEN.
- **Operational UsersRuntime shared binding:** not implemented by Generic 0.1.0; do not infer it from Users Manager.
- **Resource Preparation + startup gate:** PLANNED / DEFERRED.
- **Tool Catalog local completely filesystem:** OPEN.
- **C3/C5 qualification/evidence:** OPEN according to their owners.
- **Docker/Azure final runtime:** UNVERIFIED.
- **Materialization ↔ Web Projection Cosmos physical E2E:** UNVERIFIED.
- **Live:** NOT IMPLEMENTED.
- **Management Capture/Projection, History/Analytics:** PLANNED / SEPARATE.
- **UX and END_OF_SHIFT operational work:** OPEN / SEPARATE.
- **Python 3.14.7/Trixie migration:** BLOCKED / DEFERRED; current relevant packages still declare `==3.14.2`.
