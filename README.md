# AI Governance Map

An interactive, geographic map of global AI governance: where binding AI law
exists, where it's partial, where it's only draft or voluntary, and where
there is no AI-specific governance at all. Built for decision-makers who are
not AI or policy specialists — legible in under 10 seconds, no jargon,
traffic-light-simple categories — backed by rigorously sourced and
fact-checked data.

## Why this exists

AI governance is fragmented and moving fast. Executives, city councils, and
other non-specialist decision-makers need a single, trustworthy view of
"does my country/region have binding AI rules, and if so what do they
actually cover" — without reading legal text. This project builds that view
as a sourced dataset, independently fact-checked, then turned into an
interactive artifact.

## Pipeline

Four sequential steps. Each is designed to be picked up independently if a
session is interrupted — check the relevant directory for what's already
done before restarting a step.

1. **Research** (`/research/`) — build the raw dataset: one row per
   country/region plus international/regional bodies (EU, OECD, Council of
   Europe, UN, G7 Hiroshima Process, ASEAN, African Union, etc.), each with
   governance category, key instrument(s), scope, enforcement mechanism,
   effective date, sources, plain-language summary, and confidence.
   - `research/sources.csv` — master dataset
   - `research/notes/` — narrative notes per region, one `.md` file each
   - `research/needs-followup.md` — sources that couldn't be accessed
     (paywalled, geo-blocked, login-gated) and were not guessed from memory

2. **Fact-check** (`/fact-check/`) — independently verify every row in
   `research/sources.csv`. Not a trust-the-first-pass step: each claim needs
   at least one independent corroborating source (primary source preferred),
   a currency check (flag anything sourced >6 months old), and a fresh
   attempt at anything Agent 1 couldn't access.
   - `fact-check/verified-sources.csv` — Agent 1's schema plus
     `verification_status` and `verification_notes`
   - `fact-check/disputed.md` — claims that couldn't be confirmed or that
     conflict across sources, for manual resolution before publishing

3. **Design prompt** (`/design/`) — not the map itself. A single, complete,
   ready-to-use prompt for Claude's artifact tooling to generate the actual
   interactive choropleth map, written after reading the verified dataset.
   - `design/map-prompt.md` — the finished prompt

4. **Build the artifact** (not part of this repo's agents) — take
   `design/map-prompt.md`, review it, and hand it to Claude (with the
   `artifact-design` / dataviz skill) to generate the actual interactive map.
   This is a separate, manual step after the three agents above finish.

## Status

See each directory's files for current progress. Commits are made
incrementally per region/step, so partial progress is always recoverable —
if picking this back up, check `git log` and the `needs-followup.md` /
`disputed.md` files first to see what's outstanding.

## Data conventions

- Governance categories (5 tiers, used consistently across all outputs):
  `Comprehensive binding law` / `Sectoral or partial binding law` /
  `Draft legislation pending` / `Non-binding guidelines or voluntary only` /
  `No known AI-specific governance`
- Every claim carries a source URL, date accessed, and a confidence/
  verification rating — nothing is asserted from memory without a citation.
- Confidence ratings: `high` / `medium` / `low` /
  `unverified — source inaccessible`.
