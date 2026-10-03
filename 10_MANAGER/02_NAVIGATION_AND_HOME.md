# Manager — Navigation and Home

Estado: **CURRENT UI / ACCESS SEMANTICS REFINEMENT PLANNED**

## Manager and Navigation separation

Manager authorization remains separate from operational Navigation authorization.

## Navigation Configuration CURRENT

Persisted link contract has `allowed_profiles` and no explicit access mode.

Runtime currently does:

```text
administrative_override → allow
disabled route           → deny
allowed_profiles empty   → allow
profile in list          → allow
otherwise                → deny
```

Menu resolution uses the same empty-list-is-public behavior.

## Gap — OPEN / NEXT

UI must expose:

```text
Acceso
○ Público
○ Restringido
```

PUBLIC:
```text
ordinary profile selector empty/disabled
```

RESTRICTED:
```text
ordinary profile selector enabled
zero selections valid
```

Semantics:

```text
PUBLIC
RESTRICTED + []          root/local only
RESTRICTED + [profiles]  profiles + root/local
```

## Invariant

```text
root   assignable to USER
root   NOT selectable as Navigation route grant
local  NOT selectable as Navigation route grant
```

## Acceptance

Both menu visibility and direct URL authorization must obey the same decision.
