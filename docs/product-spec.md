# Daily Knowledge App — Product & Technical Spec (v0.1)

## 1. Vision

A web app (PWA) that delivers **one accurate, well-sourced, fun fact a day** about the world — helping users build general knowledge as a low-effort daily habit rather than a cramming exercise. Think "Wordle for world knowledge": a 30-second daily ritual that compounds over months into real, retained knowledge.

**Core principles:**
- **Low cognitive burden** — one fact, one card, minimal taps
- **Trustworthy** — every fact cites verifiable sources
- **Fun, not gamified-to-addiction** — habit reinforcement, not dark-pattern engagement
- **Agentic by design** — content is researched, verified, and curated by an AI pipeline, not manually written each day

---

## 2. Target Platform

**v1: Progressive Web App (PWA)**, installable to homescreen.

Rationale:
- Core loop (one card, optional quiz, streak counter) doesn't require native capabilities
- Web Push works on iOS 16.4+ once added to homescreen — streak reminders are achievable without native
- Service workers enable basic offline caching (today's fact loads with no signal)
- Single codebase, instant iteration, no App Store review cycle while validating the concept

**Deferred to v2 (post-validation):** Native iOS/Android app — primarily to unlock home screen widgets (fact visible without opening the app) and richer native push. Decision gate: build native only once retention data justifies the rewrite cost. Backend and agent pipeline should be built platform-agnostic from day one so this is a frontend-only decision later.

---

## 3. Content Pillars

| Pillar | Notes |
|---|---|
| Geography & capitals | Pairs well with maps/visuals |
| Food history & origins | Strong narrative hooks ("why is it called...") |
| Landmarks (ancient + modern) | Highly visual category |
| Trade & economic history | Silk Road, spice trade — connects naturally to food & geography |
| Internet culture / modern references | Fast-decaying content, needs distinct sourcing/freshness pipeline |
| *Future candidates* | Language & etymology, "on this day" history, science "how it actually works" |

**Design note:** Categories are intentionally interlinked (trade ↔ food ↔ geography) to support a "connect the dots" retention feature later.

---

## 4. Core Feature Set

### 4.1 Daily Core Loop
- One card per day: fact + 1–2 sentence "why this matters" hook + striking image
- Visible source citation on every card (trust differentiator vs. generic trivia apps)
- Optional micro-quiz: guess-before-reveal format (e.g., "Which country has the most time zones?") to convert passive reading into active recall

### 4.2 Gamification (light-touch)
- **Streaks** with a "streak freeze" allowance so one missed day doesn't reset progress
- **Knowledge map/passport** — a visual (e.g., world map) that fills in as facts are learned, tying game progress to the content theme
- **Weekly theme arcs** (e.g., "Silk Road week") instead of pure randomization, so facts build on each other narratively

### 4.3 Retention Mechanics
- **Spaced repetition resurfacing** — occasional "remember this?" prompts on facts from prior weeks
- **Connect-the-dots** — surfaces links between today's fact and a previously learned one, leveraging the interlinked category design

### 4.4 Habit Triggers (no native push in v1)
- Web Push (post-homescreen-install) as primary reminder channel
- Email digest as fallback/simplest option
- PWA offline caching ensures today's card is always available

---

## 5. Agentic Content Pipeline

**Two-agent verification pattern** to balance automation with accuracy:

1. **Researcher agent** — drafts a candidate fact with citations, pulling from trusted sources (encyclopedic references, national archives, museum sites, .gov/.edu domains)
2. **Fact-checker agent** — independently cross-verifies claims against a second, independent source before the fact is marked publish-ready
3. **Personalization agent** (later phase) — sequences/selects facts per user based on category engagement, avoiding repetition and over-indexing on ignored themes

**Category-specific handling:**
- Stable historical categories (landmarks, food history, trade) → verified against durable reference sources, long shelf life
- Internet culture/modern references → requires near-real-time web search, shorter freshness window, distinct trust/verification model from historical facts

**Pipeline timing:** Nightly batch generation (research → draft → verify → queue) rather than on-demand generation at request time. This allows a human-in-the-loop QA gate before content ships and keeps the read path fast and cheap.

---

## 6. Technical Design Notes

- **Fact bank (source of truth):** Internal database of `{fact, category, sources[], difficulty, freshness_window, image, quiz_prompt}` — enables dedup, spaced repetition, and personalization without re-querying an LLM on every read.
- **Read path vs. generation path:** Decoupled — users always read from the pre-verified fact bank; generation/verification happens offline nightly.
- **Backend/agent pipeline:** Platform-agnostic (not tied to PWA vs. native), so the v2 native decision is a frontend-only lift later.
- **Offline support:** Service worker caches current day's fact + assets for no-signal access.
- **Notifications:** Web Push API (post-homescreen install) + email fallback.

---

## 7. Suggested MVP Scope (v1)

**In scope:**
- PWA shell, installable to homescreen
- Daily fact card with image, hook, and citation
- Guess-before-reveal micro-quiz
- Basic streak counter (with freeze allowance)
- Nightly agent pipeline: researcher + fact-checker, human QA gate, fact bank storage
- 3–5 starting categories (recommend: capitals, food history, landmarks, trade history — hold internet culture for a fast-follow given its distinct pipeline needs)
- Email digest reminder

**Deferred to later phases:**
- Knowledge map/passport visualization
- Spaced repetition resurfacing & connect-the-dots
- Personalization agent
- Internet culture category (separate freshness pipeline)
- Native app + widgets

---

## 8. Open Questions for Design/Dev Phase
- **What's the minimum viable "trusted source" allowlist per category?**
  *Recommendation:* Tier sources by category rather than using one universal list —
  - **Capitals/geography:** national government sites (.gov equivalents), CIA World Factbook, UN data
  - **Food history:** established culinary/cultural encyclopedias, national archives, university food-history departments
  - **Landmarks:** UNESCO World Heritage listings, national heritage/tourism boards, museum sites
  - **Trade & economic history:** university economics/history departments, national archives, established economic-history references
  - **Internet culture:** treat separately — primary platform data (view counts, original post/video) plus 2–3 established tech/culture publications, with a short freshness window and lower long-term confidence than the historical categories
  Keep the list small (3–5 source types per category) and versioned in the fact bank schema, so the fact-checker agent has a fixed allowlist to check against rather than open web search — this keeps verification consistent and auditable. Expand the list only when a category's fact-checker rejection rate is high enough to suggest coverage gaps.
- **How is human QA staffed/scheduled in the nightly pipeline?**
  *Approach:* Human review is spot-check only, applied *after* the fact-checker agent's first pass — not a full manual review queue. The fact-checker agent itself should flag which facts need human eyes (e.g., low source agreement, ambiguous or conflicting claims, low-confidence category like internet culture) rather than sampling randomly or reviewing everything. This keeps human effort proportional to actual risk in the content rather than a fixed staffing load.
- **What's the visual identity — playful/illustrated vs. photo-realistic imagery?**
  *Direction:* Hybrid, chosen per fact rather than a single fixed style. Playful/illustrated as the default tone (keeps the app feeling fun and low-pressure), but switch to photo-realistic when the real image *is* the payoff — e.g., an actual landmark photo, a real historical artifact, or an internet-culture fact where the original image/screenshot is the point. This decision can live in the fact bank schema (an `image_style` field set during content curation) so it's an explicit per-fact choice, not left to chance.
- **Sign-up requirement:** Account-gated from day one. This simplifies streak persistence, personalization, and the email/push reminder flow from the start, rather than retrofitting accounts later once anonymous usage patterns exist.


## 9. Design artifact
https://claude.ai/code/artifact/cf1acd98-40d7-4e36-a780-ae1396d3c442
