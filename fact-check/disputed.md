# Disputed / Uncertain / Unverifiable rows

One entry per row from `fact-check/verified-sources.csv` whose
`verification_status` is `Uncertain`, `Unverifiable`, or `Disputed (sources
conflict)`. Written for the project owner to review before publishing —
each entry explains the conflict/gap in plain terms, gives both/all
conflicting claims with sources, and states a lean where I have one.

Batch A (international & regional bodies): no entries — all 8 rows verified
with 2+ independent sources.

Batch B (Americas): no entries requiring this file — 7 of 8 rows verified
with 2+ independent sources; Argentina landed at "Likely (1 source,
credible)" because one sub-claim (the exact date a minors'-AI-profiling
bill was introduced) couldn't be pinned down precisely, but this doesn't
rise to a genuine dispute or gap worth a full entry here — see
verification_notes in the CSV for that row.

Batch C (non-EU Europe): no entries — all 7 rows verified with 2+
independent sources. Notably, Norway's priority conflict (in force vs.
delayed) is now RESOLVED: confirmed delayed, not in force, bill unlikely
before the Storting until spring 2027 and entry into force unlikely before
mid-2027 — see the CSV row's verification_notes for the source.

## Bangladesh — National AI Act target date (2027 or 2028?)

**Row status:** Disputed (sources conflict)

**What's in dispute:** Bangladesh's own draft National AI Policy and
Regulatory Framework 2026-2035 contains two different target dates for its
planned National AI Act, in two different parts of the SAME document:

- One passage says: "By 2027, the National AI Act shall be enacted, the AI
  Regulatory Authority shall be operational, and the first set of delegated
  regulatory instruments under the Act shall be in force."
- Another passage directs the Ministry of Law to "initiate the drafting of
  a comprehensive Artificial Intelligence Act by 2028."

These cannot both be right: you can't have a law "enacted" and a regulator
"operational" by 2027 if the ministry responsible for drafting the law is
only instructed to START drafting it by 2028. This is not a case of two
outlets disagreeing — it's the Bangladeshi government's own draft policy
document contradicting itself.

**Sources for each reading:**
- 2027-enactment reading: draft policy text as quoted/summarized in
  https://regulations.ai/countries/bangladesh and cross-referenced against
  the official draft PDF at https://aipolicy.gov.bd/docs/national-ai-policy-bangladesh-2026-2030-draft-v2.0.pdf
- 2028-drafting-start reading: same official draft PDF, a different
  section (Ministry of Law implementation instructions)

**My lean:** This looks like a genuine drafting error in a document that
was itself still in draft/consultation form as of the research date (public
consultation on the v2.0 draft closed Feb 2026, and as of Sept 2026 the
policy still has not been formally adopted). My best guess is that the 2027
date is the more "aspirational headline" figure meant for public
communications, while the 2028 date reflects the more realistic internal
bureaucratic timeline — but this is a guess, not a finding. **This needs a
human to check the FINAL adopted policy text (once published) rather than
either draft**, since the contradiction may simply get fixed in the final
version, or one date may be dropped. Do not build a map tooltip or
visualization claim around either specific year for Bangladesh's AI Act
without flagging this uncertainty, or better, wait for the final adopted
policy.

Batch E (Middle East): no entries — all 4 rows verified with 2+ independent
sources. One correction made directly in the CSV: Saudi Arabia's new
copyright law (with its AI-training exception) enters into force 12 Aug
2026, not 1 Aug 2026 as Agent 1's row said.

## Nigeria — has the National Digital Economy and E-Governance Bill been signed into law?

**Row status:** Unverifiable

**What's in dispute:** Whether President Bola Tinubu has signed Nigeria's
National Digital Economy and e-Governance Bill 2025 — the bill that would
give NITDA real AI-regulatory powers, including fines. Agent 1 logged this
exact gap in `research/needs-followup.md` and could not resolve it. I made
a genuinely fresh, independent attempt (different search terms, tried to
fetch NITDA-adjacent primary sources, searched specifically for Aug-Sept
2026 Nigerian coverage) and STILL could not confirm the answer either way.

**What I found on each side:**
- Evidence it might still be pending: the most recent substantive coverage
  I found (13 Aug 2026, TechCabal, about a related but distinct law — the
  NIMC Act 2026 on digital identity) still describes the digital economy
  bill as separate and does not mention it as signed. Earlier reporting
  (Nov 2025-early 2026) consistently described it as passed by the
  National Assembly and awaiting transmission/assent, with lawmakers
  targeting "end of March 2026" or "Q2 2026" for enactment — targets that
  would have already passed by the Sept 2026 research date, one way or the
  other.
- Evidence it might already be signed: none found. No gazette notice, no
  NITDA/presidency press release, no Nigerian tech-press "bill signed"
  headline turned up in any search.

**My lean:** More likely NOT yet signed as of Sept 2026. My reasoning: (1)
Nigerian tech and general press was actively covering other Tinubu bill
signings in the same window (e.g. the NIMC Act 2026, signed and reported
promptly) — if the digital economy bill had been signed, similar coverage
would likely exist and be easy to find; (2) the repeated slipping of
informal targets (Nov 2025 to March 2026 to Q2 2026) suggests a bill still
working through final legislative/procedural steps rather than one that
quietly became law without notice. But this is genuinely a lean, not a
confirmed fact — **the project owner should check Nigeria's official
gazette (gazettes.ng or the National Assembly's own site) or NITDA's press
page directly before publishing anything that states Nigeria's governance
category one way or the other.** If it HAS been signed, Nigeria's category
should move from "Draft legislation pending" to "Comprehensive binding
law" or "Sectoral or partial binding law" (given the ₦10M/2%-revenue fine
structure described in the bill).

