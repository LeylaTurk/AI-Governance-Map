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

