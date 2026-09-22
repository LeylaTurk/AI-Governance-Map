# Prompt: Build the interactive AI Governance Map artifact

Copy everything below this line into a fresh Claude conversation (with the
`artifact-design` and `dataviz` skills available) as your instruction.
Everything you need is either stated explicitly here or in the referenced
CSV file.

---

## Your task

Build a single interactive **choropleth map artifact** (HTML/JS, rendered
as a Claude Artifact — not a static image, not a slide, not a PDF) showing
the state of AI governance around the world. This is for a general,
non-specialist audience: think a sector CFO or a city councilmember with no
AI-policy background, who has about 10 seconds of attention before they
decide whether to keep looking.

Before writing any code, **load the `artifact-design` skill and the
`dataviz` skill**. This project's convention (see its README) requires
both to be loaded before building any visualization artifact. Follow their
guidance on layout, typography, color tokens, dark-mode handling, and chart
construction on top of everything specified below.

## The data

The verified dataset lives at `fact-check/verified-sources.csv` in this
repository. It has 52 rows and this schema:

```
region_or_country, governance_category, key_instruments, scope,
enforcement_mechanism, effective_date_or_status, source_urls,
date_accessed, plain_language_summary, confidence, verification_status,
verification_notes
```

**Data-binding decision: inline the verified rows directly into the
artifact's own data, rather than having the artifact fetch the CSV as an
external file at runtime.** Reasoning: a Claude Artifact should be
self-contained and work the first time it's opened, with no dependency on
a file being reachable at render time or on CSV-parsing edge cases (the
`source_urls` and `verification_notes` fields contain embedded commas and
quoted sub-clauses) breaking silently in the browser. Read the CSV, parse
it once while building the artifact, and embed the resulting rows as a
JS/JSON data structure inside the page (or as a small companion JS/JSON
data file published alongside the HTML — either is fine, but do not make
the map depend on fetching the raw CSV over the network). Every value shown
in the map must come from this parsed data — do not invent, guess, or
"fill in" anything not present in the CSV.

### Real scale of the dataset (read from the actual file — do not assume different numbers)

52 total rows, breaking down by `governance_category` as:

- **Comprehensive binding law**: 5 rows — European Union, Peru, South
  Korea, Vietnam, Kazakhstan
- **Sectoral or partial binding law**: 10 rows — United States, Canada,
  Italy, China, Japan, India, Taiwan, United Arab Emirates, Qatar, Russia
- **Draft legislation pending**: 19 rows — Council of Europe, Brazil,
  Mexico, Chile, Colombia, Norway, Turkey, Ukraine, Serbia, Indonesia,
  Thailand, Philippines, Bangladesh, South Africa, Nigeria, Kenya, Egypt,
  Ghana, Morocco
- **Non-binding guidelines or voluntary only**: 17 rows — OECD, United
  Nations, G7 (Hiroshima AI Process), ASEAN, African Union, GPAI/Bletchley,
  Argentina, United Kingdom, Singapore, Australia, New Zealand, Malaysia,
  Pakistan, Israel, Saudi Arabia, Rwanda, Tunisia
- **No known AI-specific governance**: 1 row — Switzerland

Of these 52 rows, **8 are international/regional bodies, not individual
countries**: European Union, OECD, Council of Europe, United Nations, G7
(Hiroshima AI Process), ASEAN, African Union, and Global Partnership on AI
(GPAI) / Bletchley Declaration & AI Safety Summit process. That leaves
**44 rows that are individual countries**, to be shaded directly on the
choropleth. See point 9 below for exactly how to handle the 8
international-body rows, since they don't map onto contiguous national
territory the same way.

All 52 rows have `date_accessed` of **2026-09-22** — the entire dataset
was verified as of that single date, which simplifies the provenance
statement (see point 10): there's no need to show per-row verification
dates, just the one dataset-wide date.

## Required specification — hit every point below explicitly

### 1. Map type
An interactive choropleth: countries/regions are filled with color based on
`governance_category`, and the viewer can pan/zoom or at minimum
hover/click individual countries. Build it as a real Claude artifact
(HTML with inline JS/CSS, or a small set of published files) — not a
flat image, not a screenshot, not a chart library's static export. Use an
existing lightweight geographic library/approach compatible with an
Artifact's constraints (e.g. an inline world TopoJSON/GeoJSON + SVG
path rendering, or a CDN-loaded mapping library permitted by
`artifact-design`'s allowed script sources) — do not attempt to hand-draw
country borders yourself.

### 2. Primary legend — governance status (5 tiers, ordered, colorblind-safe)

Exactly 5 tiers, matching `governance_category` values verbatim, in this
order from most to least governed (this is an **ordered/sequential** scale,
not a nominal categorical one — treat it that way in your color choice):

1. `Comprehensive binding law` — deep green
2. `Sectoral or partial binding law` — yellow-green
3. `Draft legislation pending` — yellow/amber
4. `Non-binding guidelines or voluntary only` — orange
5. `No known AI-specific governance` — red

Use a colorblind-safe sequential/diverging palette appropriate for 5
ordered classes — for example ColorBrewer's `RdYlGn` (5-class) scheme,
which is built for exactly this kind of "good to bad" ordered data and is
documented as colorblind-safe in its 5-class configuration:

- Comprehensive binding law: `#1a9850`
- Sectoral or partial binding law: `#91cf60`
- Draft legislation pending: `#fee08b`
- Non-binding guidelines or voluntary only: `#fc8d59`
- No known AI-specific governance: `#d73027`

(If you use `dataviz` skill's own validated palette utilities instead,
that's fine — just keep the same ordering logic: green-to-red, 5 steps,
verified colorblind-safe, and do not substitute a nominal/categorical
palette where hue doesn't encode order.)

**Color must never be the sole signal.** Pair each tier with a distinct
icon/symbol shown in the legend, in the country card, and — space
permitting — as a small glyph or hatch pattern on the map fill itself, so
the map still communicates correctly in grayscale or for colorblind
viewers. Suggested symbol set (adjust to taste, but keep them visually
distinct at small size): a filled circle/checkmark for comprehensive law,
a half-filled circle for sectoral, a pencil/draft icon for pending
legislation, a speech-bubble/handshake icon for voluntary guidelines, and
an empty circle/dash for no governance. A 6th, visually distinct
treatment (see point 7) is required for "not yet assessed" — do not let it
share a symbol with "no known governance."

### 3. Gap layer (toggle or secondary view)

Add a toggle (e.g. a switch or second view mode) that switches the map from
"where governance exists" (the tier-based fill above) to a **gap
highlight** view. Define "gap" concretely and mechanically from the CSV
columns so nothing is left to interpretation:

A country/region counts as a **governance gap** if ANY of the following is
true, using the exact `verification_notes`/CSV columns as the source of truth:

- `governance_category` is `No known AI-specific governance` (Switzerland is
  the only current example), OR
- `governance_category` is `Non-binding guidelines or voluntary only` AND
  `enforcement_mechanism` contains no binding enforcement for the general
  population (i.e., the field's text indicates "no", "voluntary", or
  "no enforcement" rather than describing a binding mechanism — most rows
  with this category already say this explicitly, e.g. UK, Argentina,
  Singapore), OR
- `enforcement_mechanism` text itself signals no enforcement regardless of
  category (look for enforcement_mechanism values starting with "no —" or
  containing "not yet" / "no enforcement" / "voluntary" / "no fines"), OR
- `scope` text indicates coverage that explicitly excludes or doesn't reach
  high-risk uses (e.g. narrow sector-only scope such as Argentina's
  provincial-only coverage, or Turkey's single narrow automated-decision
  right) even where some binding law exists elsewhere in that country's row.

In the gap view: shade "gap" countries in a single consistent warning
color (e.g. a hazard-orange/red, visually distinct from tier 5's red so
the two views aren't confused — consider a hatch/diagonal-stripe fill to
reinforce this is a different lens than the primary legend), and mute
(gray out) every other country. Add a one-line caption above the map when
this toggle is active, e.g. "Showing where AI is used in high-risk ways
with no binding rules or enforcement behind it" so the non-specialist
audience isn't left to infer the toggle's meaning.

### 4. Interactivity — hover/click country card

On hover (desktop) and tap (mobile/touch), show a card with, in this
order:

- **Status in plain words** — do not show the raw `governance_category`
  string verbatim as the headline; translate it to a short plain phrase,
  e.g. `Comprehensive binding law` → "Binding AI law in force",
  `Sectoral or partial binding law` → "Some binding AI rules (partial)",
  `Draft legislation pending` → "AI law proposed, not yet passed",
  `Non-binding guidelines or voluntary only` → "Voluntary guidelines only,
  no binding law", `No known AI-specific governance` → "No AI-specific
  rules found". Keep the underlying `governance_category` value available
  in a data attribute/tooltip for anyone who wants the precise term, but
  the plain phrase is what's visually prominent.
- **Key law name(s)** — from `key_instruments`, shown as a short list, not
  the full legalese description (you may trim to the instrument's name and
  a 3-6 word parenthetical, dropping subsection/article-level detail from
  that field where it's long).
- **The 1-sentence summary** — use the `plain_language_summary` field
  **verbatim or with only trivial trimming for card length**. Do not write
  new explanatory copy from scratch — that field already exists precisely
  so you don't have to invent plain-language framing.
- **Effective date/status** — from `effective_date_or_status`, trimmed to
  its headline fact if the field is long (e.g. lead with the actual
  in-force date or "not yet in force" / "pending" framing, rather than
  the full multi-clause legal history — the full text can go in an
  expandable "more detail" section of the card if you want one).
- **A clickable source link** — use the first URL in `source_urls` (it's a
  semicolon-separated list; the first entry is fine as the primary link,
  opened in a new tab) labeled something plain like "View source" rather
  than the raw URL.

**No legal jargon anywhere in the visible UI.** Headlines, labels, and
buttons should all read in plain English. Legal instrument names (which
are unavoidably technical) are fine inside the card body since they're
labeled as "law name," but nothing legalistic belongs in headline copy,
legend labels, or button text.

### 5. Audience framing — legible in under 10 seconds, no legend lookup

The non-specialist viewer must be able to look at the map and understand
its meaning without hunting for a legend. Concretely:

- Put **one short framing sentence at the very top of the page**, above
  the map, stating what they're looking at in plain terms — e.g. "Where
  in the world does binding AI law actually exist? Darker green = stronger
  binding rules, red = none." This sentence itself should teach the
  color logic so no separate legend lookup is required to get the gist.
- Keep the 5-tier legend visible on-screen at all times (not hidden behind
  a click), compact, with short plain labels (use the plain-phrase
  versions from point 4, not the raw category strings) directly next to
  each color swatch.
- Use the intuitive traffic-light convention (green = good/governed,
  red = ungoverned) consistently — never invert it — since that mapping is
  itself doing a large share of the "no legend needed" work.
- Avoid unnecessary chart chrome, dense text blocks, or anything that
  requires reading before the color pattern itself is meaningful.

### 6. Accessibility

- **WCAG-compliant contrast**: legend text, card text, and UI chrome must
  meet at least WCAG AA contrast ratios (4.5:1 for normal text, 3:1 for
  large text/UI components) against their backgrounds in both light and
  dark mode.
- **Mobile width**: the page must work with no horizontal scrolling and
  legible tap targets down to **375px viewport width** (iPhone SE/standard
  small-phone breakpoint). At that width, the legend and country card
  should stack below or below the map rather than sitting in cramped
  side-by-side columns; the map itself should remain pannable/zoomable
  with touch gestures.
- **Never rely on color alone**: implement the icon/symbol-per-tier system
  from point 2 consistently across the legend, the map fill (as an overlay
  glyph or hatch pattern, at least at hover/zoomed-in level if not
  always-on at world-view zoom), and the country card's status line, so
  the map remains usable in grayscale printing and for colorblind viewers.

### 7. Countries not covered by the dataset ("not yet assessed")

The dataset covers 44 individual countries out of the world's roughly 195.
**Every other country must still appear on the map**, shaded in a
visually distinct "not yet assessed" treatment — a neutral light gray or
hatched pattern, clearly different from tier 5's red ("No known AI-specific
governance"). Do not omit uncovered countries from the map, and do not let
them look identical to a researched "no governance" verdict — those are
different claims (one means "we checked, there's nothing"; the other means
"we haven't checked this country yet"). Clicking/hovering an unassessed
country should still show a small card, but one that plainly says
something like "Not yet assessed in this dataset" rather than mimicking
the format of an assessed country's card (no fabricated status, no
fabricated summary).

### 8. Disputed/uncertain rows — Bangladesh and Nigeria

Both `fact-check/disputed.md` and the CSV's `verification_notes` column
flag two rows that must **not** be silently resolved one way or the other
in the map UI:

- **Bangladesh** (`verification_status`: Disputed — sources conflict):
  its own draft National AI Act policy self-contradicts on whether the Act
  is targeted for 2027 or 2028.
- **Nigeria** (`verification_status`: Unverifiable): it could not be
  confirmed whether its Digital Economy and e-Governance Bill has actually
  been signed into law.

For these two rows specifically (and generally, any row whose
`verification_status` is not `Verified (2+ independent sources)` —
Argentina is `Likely (1 source, credible)` and should get a lighter-weight
version of this treatment too):

- Show a small, visually distinct **flag/caveat indicator** on the
  country's map fill itself (e.g. a small "!" badge or asterisk overlay on
  the country shape) so a viewer sees the uncertainty before they even
  open the card — don't make it discoverable only after clicking.
- Inside the country card, add a clearly labeled caveat section (e.g. a
  bordered callout headed "Flagged: unresolved detail" or similar) that
  states in plain language what's uncertain and why, drawn from that row's
  `verification_notes` field — do not just show the raw verification_notes
  text as-is (it's written for a researcher audience); write one or two
  plain sentences that capture the actual uncertainty (e.g. for Bangladesh:
  "This country's own draft AI policy gives two different target dates —
  2027 in one section, 2028 in another — and hasn't been resolved yet.");
  for Nigeria: "It's unclear whether this bill has actually been signed
  into law yet — reporting is mixed and no official confirmation was
  found."
- Do not pick a single date/status for Bangladesh or a single yes/no for
  Nigeria and present it as settled anywhere in the visible UI — the map's
  fill color for these two should reflect their current logged
  `governance_category` (`Draft legislation pending` for both) since
  that's the verified/agreed category even though a sub-detail is
  disputed, but the caveat badge must still be visibly present.

### 9. International and regional bodies (8 rows, not individual countries)

The 8 rows — European Union, OECD, Council of Europe, United Nations, G7
(Hiroshima AI Process), ASEAN, African Union, and GPAI/Bletchley — need
different treatment from the 44 country rows, since most don't correspond
to contiguous national territory:

- **European Union**: shade all 27 EU member-state territories with the
  EU's tier color and data (unless a member state has its own row in the
  dataset with a different/more specific category — check the data: Italy
  has its own row as `Sectoral or partial binding law`, which reflects
  Italy-specific AI law layered on top of the EU AI Act, so Italy should
  show its own row's data, not the EU's, when hovered; every other EU
  member state without its own row should inherit the EU row's data and
  color). Clicking any EU member state's territory shows the EU row's
  card content (or Italy's own, for Italy).
- **The other 7 bodies** (OECD, Council of Europe, UN, G7 Hiroshima, ASEAN,
  African Union, GPAI/Bletchley) are multilateral/non-geographic or
  span too many disconnected territories to sensibly shade on a
  choropleth (e.g. OECD and the UN span most of the world and would just
  paint over country-level detail; ASEAN and the African Union are
  regional blocs whose member countries mostly also have their own
  individual rows already). **Do not force these onto the choropleth
  fill.** Instead, give them a separate small panel or list — e.g. a
  collapsible "International & regional frameworks" section below or
  beside the map — showing each body's name, plain-language one-line
  status, and the same card detail (key instrument, summary, source link)
  as the country cards, just laid out as a list rather than a map fill.
  Make clear in a one-line intro to that panel that these apply across
  many countries at once rather than to a single territory.

### 10. Provenance / trust signal

Add a visible "About this data" affordance (a footer line, an info icon
with a small popover, or a dedicated collapsible section — your choice of
placement, but it must be reachable without leaving the page) stating,
in plain language:

- This data was **independently fact-checked** (not just single-pass
  research) — every claim was checked against at least one additional
  source before publishing, with a small number of exceptions that are
  flagged rather than hidden (link or refer to the Bangladesh/Nigeria/
  Argentina caveats here too, so a skeptical viewer immediately knows those
  exist).
- **Last verified: September 22, 2026** — use this actual date (it's the
  `date_accessed` value for all 52 rows in the current dataset; if you are
  building from an updated CSV with different dates later, use whatever
  date(s) actually appear in that file instead of this one).
- A pointer back to the methodology: state that the underlying research
  and independent fact-check are maintained in this project's repository,
  specifically its `/research` (raw sourced dataset, one row per
  country/body) and `/fact-check` (independent verification pass,
  `verified-sources.csv` and `disputed.md`) folders, so a skeptical viewer
  can trace any claim on the map back to its original source citation.

## Final check before you consider this done

- Every country/region shown with a governance-tier color must trace back
  to an actual row in `fact-check/verified-sources.csv` — no invented or
  extrapolated statuses.
- Every country not in the dataset is visibly present and marked "not yet
  assessed," never blank/missing and never colored as if it were
  researched.
- Bangladesh and Nigeria (and Argentina, more lightly) carry a visible
  caveat indicator, not a silently resolved status.
- The 5-tier legend, the gap-layer toggle, the country card fields, the
  colorblind-safe palette + icon system, the mobile breakpoint, the
  international-bodies panel, and the provenance footer are all present
  and match the specifications above.
