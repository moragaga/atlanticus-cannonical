# ADA Command Center — Identity, Users, Profiles, Navigation and Manager

Estado: **CURRENT — DURABLE ADMINISTRATION STORES IMPLEMENTED; PRODUCTION IDENTITY OPEN**

## Identity

Current Generic host uses `LocalIdentityProvider`.

Production Entra binding remains:

```text
PLANNED / UNVERIFIED
```

Do not infer production permissions from local administrative override.

## Users

Generic Atlanticus capability.

Current local provider:

```text
in-process registry + administration
```

Current durable provider:

```text
Blob Users Registry
Cosmos Users Runtime/administration
```

Users does not own ADA Access.

## Profiles

Generic Atlanticus capability.

Current local:

```text
shared local Source
in-process Projection
```

Current durable:

```text
Blob Source
Cosmos Profiles Projection
```

## Navigation

Generic Atlanticus capability.

Current local:

```text
shared local Source
in-process Projection
```

Current durable:

```text
Blob Source
Cosmos Navigation Projection
```

Operational navigation consumes the same projection store supplied by administration.

## Manager authorization — FROZEN

`atlanticus-web-manager==0.3.19`.

Local Command Center principal:

```text
subject_id=<local identity subject>
profile_keys=('local',)
administrative_override=True
is_local=True
```

## Master Projection interaction

Command Center Master Projection reuses the Profiles and Navigation projection stores and includes
Alarm Configuration as a third domain.

It does not create a parallel Manager.

## OPEN

```text
Production Entra provider
production authorization mapping
real durable runtime smoke
Command Center Source/namespace generic ownership cleanup
```
