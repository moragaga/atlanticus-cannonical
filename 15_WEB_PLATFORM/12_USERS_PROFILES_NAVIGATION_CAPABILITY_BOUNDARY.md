# Web Platform — Users / Profiles / Navigation / Manager Capability Boundary

Estado: **CURRENT IMPLEMENTATION + DECIDED ADA TOOL-SCOPED CUTOVER**

## Generic ownership remains

```text
Atlanticus Users       identity/lifecycle contracts
Atlanticus Profiles    profile definitions/catalog
Atlanticus Navigation  route structure/authorization
Atlanticus Manager     administrative shell/authorization
ADA Access             ADA operational profile -> access_keys
ADA product            composition
```

Generic packages do not gain ADA-specific dependencies.

## System profiles

```text
basic
root
guest
local
```

`root` and `local` remain privileged identities/profiles.

They are not normal editable grants in Navigation.

## CURRENT implementation

Global `UserRecord` currently includes:

```text
profile_key
enabled
```

ADA Navigation profile options intentionally exclude `root` and `local`.

Navigation authorization currently interprets:

```text
allowed_profiles=[]
→ unrestricted/public route
```

This implementation is valid evidence of current behavior but is not the accepted target.

## DECIDED target — Global Users

Global Users becomes identity-only:

```text
user_id
issuer
subject_id
display_name
email
```

No `profile_key`.
No Tool-specific `enabled`.

## DECIDED target — Tool User Membership

Tool scope owns:

```text
user_id
profile_key
enabled
```

This is the durable relation between global identity and one Tool.

## DECIDED target — users-runtime

Tool Cosmos `users-runtime` becomes the complete authority for session read of that Tool.

It contains identity + enabled + resolved profile + resolved operational values.

Access keys are not copied per user; runtime resolves them from the Access projection/cached profile mapping.

## Navigation authorization refinement

Required behavior:

```text
PUBLIC
    ordinary public route

RESTRICTED + no ordinary profile
    root/local only

RESTRICTED + profiles
    selected profiles + root/local
```

`root/local` remain implicit.

The exact schema field can be finalized in implementation, but the semantic distinction public vs privileged-only is frozen.

## Manager separation

Manager administrative authorization remains separate.

Do not map ADA Access into `ManagerPrincipal.access_keys`.

Do not use Navigation visibility as Manager authorization.

## Clean cutover

When implemented:

```text
remove global profile/enabled ownership
do not retain compatibility UserRecord shape
do not dual-write old/new membership
do not expose root/local as ordinary profile selectors
```
