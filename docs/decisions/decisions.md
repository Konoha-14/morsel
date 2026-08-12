# Decisions index

The chronological log of architecture/product decisions. Each entry links to a
`decision-<nnn>.md` record. RULE: every decision needs a paper trail.

Seed decisions carried over from the architecture paper (`../architecture.md` §7):

| ID | Decision | Status |
|----|----------|--------|
| D1 | Global daily fact for v1 (not personalized) | accepted |
| D2 | Deterministic control flow, agentic steps | accepted |
| D3 | Persist per stage; DB is the orchestrator's memory | accepted |
| D4 | Verifier is source-blind | accepted |
| D5 | Sources are rows, not a JSON blob | accepted |
| D6 | Over-provision candidates (N per slot) | accepted |
| D7 | Image is a branch, not an agent; attribution mandatory | accepted |
| D8 | Email digest first, Web Push fast-follow | accepted |
| D9 | Reuse the known stack (FastAPI + SQLModel + Postgres) | accepted |

New decisions taken through the-loop get their own `decision-<nnn>.md` and a row here.
Conflicts between instruction sources are logged in [`conflicts.md`](conflicts.md).
