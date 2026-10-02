# ADA Command Center — Web Application

Estado: **CURRENT — GENERIC PRODUCT ROOT / LOCAL HOST SUPPORTS LOCAL OR DURABLE PERSISTENCE**

## Product roles

```text
ada-command-center-generic-application
→ real product composition root

ada-command-center-configuration-manager
→ reusable configuration/administration composition
→ separate qualification/development application
```

## Local host

```text
ManagerConfigurationReader
→ select local|durable manager provider
→ LocalIdentityProvider
→ product application
```

Production still fails fast because Generic does not inject a production-ready IdentityProvider.

## Persistence

`ATLANTICUS_ENVIRONMENT=local` is compatible with:

```text
manager_provider=local
manager_provider=durable
```

Azure versus emulator is selected by connection values, not by a different architecture mode.

## Master Projection

Independent routes are registered when a material reader is supplied.

Reader selection follows persistence:

```text
local provider
→ LocalMasterMaterialReader

durable provider
→ BlobMasterMaterialReader
```

Backend composition:

```text
Profiles
Navigation
Alarm Configuration
```

## Invariants

- Command Center is a separate product from ADA.
- ADA Access/KPI/Collector do not enter by symmetry.
- Configuration Manager is not a remote service.
- Master Projection engine remains generic Atlanticus.
- production identity is not inferred from local qualification.
