# Artifact and Distribution Boundary

Estado: **CURRENT — THREE WEB PROFILES QUALIFIED TO THEIR DECLARED CONTRACTS**

Implementation:

```text
moragaga/atlanticus@2dc5862f634eb0bf8fe72d771d56605d1c7f32cf
```

## Profiles

### Generic

```text
starter               PASS
wheelhouse packages   36
qualification         PORTABLE / PASS
readiness             ready
```

### ADA

```text
starter               PASS
internal wheels       71
distribution          BUILT_UNQUALIFIED
qualification         PRECHECK_PASS
runtime               UNVERIFIED
image_build           UNVERIFIED
```

### Command Center

```text
starter               PASS
wheelhouse packages   85
dependency_check      PASS
qualification         PORTABLE / PRECHECK_PASS
runtime               UNVERIFIED
```

## Shared boundary

```text
SOURCE PACKAGES
    ↓
PRODUCT COMPOSITION
    ↓
SHARED GENERATION / DISTRIBUTION ENGINE
    ↓
PRODUCT-OWNED STARTER OVERLAY
    ↓
ARTIFACT
    ↓
HOST / DEVOPS / RUNTIME
```

## Wheelhouse portability

The shared builder prefers a compatible locked wheel.

If no compatible wheel exists:

```text
SHA256-locked sdist
→ verify source integrity
→ PEP 517 wheel build
→ build dependencies constrained by version + SHA256 hashes
→ record source hash and final wheel hash
```

This path was verified on macOS for the dependency case that blocked `rcssmin==1.2.2`.

## Qualification semantics

`PASS` and `PRECHECK_PASS` are not interchangeable.

```text
PASS
→ declared runtime/portable probe completed

PRECHECK_PASS
→ artifact/dependency/build preconditions completed
→ runtime may remain UNVERIFIED
```

## Git traceability

Final deliverable artifacts should be regenerated after the implementing commit is published so `source_git_head` matches the authoritative commit.

## OPEN

```text
ADA runtime/image qualification
Command Center runtime qualification
dual application Storage-final + Cosmos-local smoke
Azure/Entra
```
