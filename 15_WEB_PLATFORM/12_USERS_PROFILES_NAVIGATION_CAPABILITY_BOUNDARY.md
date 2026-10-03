# Web Platform — Users / Profiles / Navigation / Manager Capability Boundary

Estado: **CURRENT USERS CUTOVER / NAVIGATION REFINEMENT PLANNED**

## Generic ownership

```text
Atlanticus Users       identity/membership/runtime
Atlanticus Profiles    profile definitions/catalog
Atlanticus Navigation  route structure/authorization
Atlanticus Manager     administrative shell/authorization
ADA Access             ADA profile -> operational access_keys
ADA                    composition
```

## System profiles

```text
basic
root
guest
local
```

Managed Tool users:

```text
root   assignable
guest  not assignable
local  not assignable
```

Navigation route grants:

```text
root   not selectable
local  not selectable
```

Root/local are implicit privileged identities for Navigation, not ordinary grants.

## Global Users CURRENT

```text
user_id
issuer
subject_id
display_name
email
```

No Tool profile/enabled state.

## Tool Membership CURRENT

```text
user_id
profile_key
enabled
```

## users-runtime CURRENT

```text
identity
enabled
profile
operational
```

Complete Tool session-read authority.

One Cosmos belongs to one Tool, so no app/tool routing fields are added.

## Local users

Jane/John use RuntimeUser-compatible session behavior.

Their avatar palette is local subject-specific presentation.

## Navigation CURRENT

Persisted config has `allowed_profiles` only.

Empty profiles means unrestricted.

## Navigation target

```text
PUBLIC
RESTRICTED + []
RESTRICTED + [profiles]
```

Manager UI must edit the mode explicitly.

## Manager separation

Do not map ADA Access into Manager authorization.

Do not use Navigation visibility as Manager authorization.

## Clean cutover

Replace ambiguous Navigation semantics cleanly; no hidden compatibility interpretation.
