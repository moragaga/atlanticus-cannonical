# ADA Web — Session, Tool User Snapshot and Operational Consumption

Estado: **CURRENT SESSION CONTRACT / REVOCATION HARDENING OPEN**

## Session CURRENT

```text
identity provider
→ users-runtime.resolve(identity)
→ RuntimeUser
→ UsersRuntime session
```

Supersedes login-time joins.

## RuntimeUser CURRENT

```text
identity
enabled
profile
operational
```

Operational has area/position/group with null when unconfigured.

## Access

ADA Access remains separate:

```text
profile_key -> access_keys
```

## Manager

Manager principal derives from RuntimeUser.

Trusted local is an explicit local-environment exception.

## Local presentation

Jane/John may use local subject-specific avatar palettes.

## Warmup

Profiles/Operational warmup is not required for login.

## Recovery

```text
Tool recovery snapshot
→ users-runtime
```

## OPEN

```text
server-side immediate session revocation
multiworker revocation propagation
production Entra qualification
```
