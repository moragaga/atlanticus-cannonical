# Manager — Bootstrap and Access

Estado: **CURRENT — RUNTIMEUSER BINDING CLOSED / NAVIGATION ACCESS SEPARATE**

## Contracts

```text
IDENTITY != USERS RUNTIME
USERS RUNTIME != ADA ACCESS
ADA ACCESS != MANAGER ACCESS
NAVIGATION AUTHORIZATION != MANAGER AUTHORIZATION
```

## ADA managed user CURRENT

```text
identity provider
→ users-runtime
→ RuntimeUser
→ ManagerPrincipal
```

Managed root produces administrative override.

Disabled RuntimeUser is rejected.

## Trusted local CURRENT

Jane/John local identities may bootstrap only in local environment.

```text
profile_keys=('local',)
administrative_override=True
is_local=True
```

Avatar palette resolves by recognized local subject.

## ADA Access

Remains:

```text
profile_key -> operational access_keys
```

Do not copy into Manager access keys.

## OPEN

```text
production Entra
multiworker/session revocation hardening
Navigation PUBLIC/RESTRICTED
```
