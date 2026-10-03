# Artifact and Distribution Boundary

Estado: **CURRENT CONTRACT / CURRENT-HEAD REGENERATION PLANNED**

Web Distribution and Process Distribution remain separate.

## Historical evidence

Previous ADA artifact built and ran in isolated Linux/Docker consumer.

That evidence does not qualify the current Users/Tool cutover HEAD.

## Current-head gate

Close first:

```text
Navigation PUBLIC/RESTRICTED semantics
```

## Planned artifact qualification

```text
generate every expected artifact
verify expected package/file set
verify metadata/dependencies
verify installability/consumer use
detect stale contents
```

## Then distribution

```text
internal wheels
+ pinned external requirements
+ external Linux image build
```

macOS rcssmin limitation remains separate.
