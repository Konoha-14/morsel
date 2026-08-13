---
type: execution-log
workItem: ""
phase: requirements-definition
status: in-progress
---

# Execution Log: `core` — Fact Bank schema, content state machine & source allowlist

> Append-only log of progress. Doubles as the resume anchor for context resets.

## Phase transitions

| Phase | Entered | Reviewed/approved by | Notes |
|-------|---------|----------------------|-------|
| requirements-definition | 2026-08-13 |  | Draft authored pre-ticket in docs/specs/draft-core-fact-bank/ |
| design |  |  |  |
| test-planning |  |  |  |
| tasks-breakdown |  |  |  |
| implementation |  |  |  |
| verification |  |  |  |
| needs-review |  |  |  |
| complete |  |  |  |

## Pull requests

| PR | Scope / tasks | Status |
|----|---------------|--------|
|    |               | open \| merged \| closed |

## Progress entries

### 2026-08-13 — Requirements drafted (pre-ticket)

- **Phase:** requirements-definition
- **Did:** Scoped the first work item as `packages/core` (the shared contract). Authored
  `requirements.md`: Fact Bank schema (R1), lifecycle state machine + legal transitions
  (R2), versioned source allowlist validated by query (R3), migrations + seed (R4), NFRs,
  and the Security considerations section (schema stores untrusted content inertly, fail-closed
  allowlist, transition guard blocks PUBLISHED bypass).
- **Checkpoint/tests:** n/a (no code yet). `.the-loop` configs validate against schema.
- **Next:** Human review of requirements. On approval, run `/the-loop:create-ticket` to open
  the GitHub issue, promote the draft folder to `docs/specs/<id>/`, then `phase-selection`.
- **Blockers:** Awaiting requirements review/approval (paper trail on the ticket once opened).

## Verification results

| What was verified | Command | Outcome | Evidence |
|-------------------|---------|---------|----------|
|                   |         | pass \| fail | link or `evidence/<file>` |

## Review cycles

| Cycle | Type (self/critic/security) | Reviewer | Outcome | Link |
|-------|-----------------------------|----------|---------|------|
|       |                             |          |         |      |

## Security review (gate)

- **Mechanism:** the-loop checklist (`security.review.mechanism: auto` → checklist when no skill)
- **Outcome:** _pending_
- **Human sign-off:** _tbd (risk tier decided at phase-selection; likely below humanSignOffMinTier=4)_

## Final validation evidence

_Pending implementation + verification._

## Capability docs

| Capability doc | What changed | History row |
|----------------|--------------|-------------|
|                |              |             |

## Documentation

| Document | What changed |
|----------|--------------|
|          |              |
