# ADA Command Center — Open Items

Estado: **CURRENT — MANAGER CONVERGENCE CLOSED LOCALLY / GENERIC APPLICATION COMPOSITION NEXT / RESOURCE PREPARATION DEFERRED**

## Authority checkpoint

```text
Last confirmed Atlanticus HEAD:
moragaga/atlanticus@36361dd570f86e8350ea4a6ee0e09bab351ba171

Command Center Manager convergence delta:
VERIFIED LOCAL / PENDING FINAL GIT HEAD
```

## CLOSED during Manager convergence

```text
atlanticus-web-manager==0.3.19
→ CURRENT authority

ADA Navigation Manager reusable adoption
→ CLOSED / VERIFIED

Command Center Alarm Configuration Manager alignment
→ CLOSED locally / VERIFIED

Command Center Tool Catalog Manager alignment
→ CLOSED locally / VERIFIED

temporary Command Center Configuration Manager alignment
→ CLOSED locally / VERIFIED
```

Local qualification of the Command Center delta:

```text
Alarm Configuration       124 PASS
Tool Catalog Manager        9 PASS
Configuration Manager host 28 PASS
Ruff                        PASS
git diff --check            PASS
Manager 0.3.18 rg           EMPTY
```

## NEXT único

```text
ADA-COMMAND-CENTER-GENERIC-APPLICATION-COMPOSITION
PLANNED / NEXT
```

Reason:

Command Center still has no accredited generic/final Web composition root equivalent to ADA Generic. The existing `ada-command-center-configuration-manager` is a temporary standalone Manager host.

The next chat must stay in design/debate first and determine from existing code:

```text
product application boundary
composition root
runtime/entrypoint
which current capabilities are mounted
Manager surface integration
whether a minimal Home is actually required
which providers/stores are required
what remains temporary
qualification contract
```

Do not start dual-product tooling/distribution before this application exists and is qualified.

## OPEN after NEXT

| Element | Estado | Motivo |
|---|---|---|
| Final Git HEAD for the locally qualified Command Center Manager delta | OPEN | Tests are green locally but this chat did not receive a post-integration HEAD. |
| Command Center Generic Application | PLANNED / NEXT | No accredited composition root/runtime exists yet. |
| Dual-product tooling/distribution | PLANNED / BLOCKED | Requires a real Command Center Generic product target first. |
| Resource Preparation + startup gate | PLANNED / DEFERRED | Important, but not the immediate next focus. |
| Users/Profiles/Navigation integration in Command Center | PLANNED / NEED-DRIVEN | Reusable compositions exist, but current temporary host does not require them. |
| Tool Catalog local completely filesystem | OPEN | Temporary local host still requires Storage for confirmed catalog. |
| C3 qualification GREEN real | BLOCKED BY DESIGN | Definitive producer/verifiers not closed. |
| C5 technical evidence/env | PLANNED | Final owner/key/version still pending. |
| Docker/runtime final Command Center | UNVERIFIED | No final integrated generic application artifact exists. |
| Alarm Source/Projection physical E2E | UNVERIFIED | Existing contracts do not prove the same physical resource end-to-end. |
| Live Projection | PLANNED | Not implemented. |
| Management Capture/Projection | PLANNED / SEPARATE | Do not mix with application composition. |
| History/Analytics | PLANNED / SEPARATE | Do not infer from FACTS/CURRENT. |
| END_OF_SHIFT operational resolution | PLANNED / UNVERIFIED | Real shift-end source/consumer remains unresolved. |
| Existing Alarm authoring UX defects | OPEN / SEPARATE | Value loss/validation/alert findings belong to another focus. |
| Python 3.14.7 / Trixie migration | BLOCKED / DEFERRED | Reopen only with explicit authorization. |

## SUPERSEDED / refined sequencing

The sequence:

```text
Manager convergence
→ immediately dual-product tooling/distribution
```

is **SUPERSEDED** because Command Center has no final generic application to distribute.

CURRENT sequence:

```text
Manager convergence                               CLOSED
ADA Generic adoption                              CLOSED
Command Center existing Manager component alignment CLOSED locally
Command Center Generic Application                NEXT
dual-product tooling/distribution                  AFTER
Resource Preparation / startup gate                LATER
```

The old file generated outside Git:

```text
ada_command_center_foundation_increment.zip
```

remains:

```text
SUPERSEDED / DO NOT APPLY
```

## Do not open during the next focus

```text
Live
Analytics
History
Management Capture
Alarm Engine redesign
new identity system
Python/Trixie migration
new compatibility aliases
new configuration contracts without an observed need
```
