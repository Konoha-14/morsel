# Architecture index

The living, organized view of Morsel's architecture. The-loop maintains this index;
per-work-item design details live in `docs/specs/<id>/design.md` and are rolled up here.

## Source of truth

The full architecture paper is [`../architecture.md`](../architecture.md) — the two-plane
design (Content Plane ↔ Serving Plane), the Fact Bank membrane, the deterministic
coordinator + agentic steps, and decisions D1–D9.

## Components

_Filled in as work items land. Start from the architecture paper's §5 Component Detail._

- Coordinator (control plane, no LLM)
- Researcher agent · Verifier agent
- Image step · Human QA (flag-only)
- Scheduler / editorial calendar
- Bake job + serving (read API, CDN `daily.json`)

## Decisions

Cross-cutting decisions are recorded under [`../decisions/decisions.md`](../decisions/decisions.md).
