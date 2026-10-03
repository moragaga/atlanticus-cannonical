# Atlanticus Web Platform — Canonical Index

Estado: **CURRENT — Users cutover y Navigation access refinement implementados; Command Center consumer parity NEXT**.

## Generic capabilities CURRENT

```text
Source
Projection
Users
Profiles
Navigation
Manager
Master Projection
Storage Namespace
Storage Topology
```

## Users CURRENT

```text
Global identity
Tool membership
RuntimeUser
runtime/recovery separation
```

## Navigation CURRENT

Persisted route access now distinguishes:

```text
PUBLIC
RESTRICTED + allowed_profiles
```

La semántica anterior "empty allowed_profiles = public/unrestricted" como único mecanismo está **SUPERSEDED**.

## Primary current gap

```text
COMMAND-CENTER-CAPABILITY-PARITY
PLANNED / NEXT
```

ADA ya consume el modelo genérico actual. Command Center aún tiene wiring anterior y bloquea el qualifier del cutover `ada-contracts`.

## After parity

```text
resume ada-contracts qualification
artifact generation
.env.detail audit
distribution regeneration
consumer runtime qualification
```
