# SHARED SPEC — ROUND 2 (the law for all 6 concepts)

**Astrus Submission Summary** — a beautiful, state-of-the-art, highly-informative
"at-a-glance" card pinned to the top of an Astrus insurance submission in Salesforce.
Pure visual mockups, each realistically buildable as a Lightning Web Component.
**LIGHT MODE ONLY.** All 6 concepts render the SAME submission (A&B Painting SUB 022667)
so they compare cleanly.

> This file supersedes round-1 `SHARED-SPEC.md`. Sources: `mission.v2`, the priority pack
> (priorities-analyst, grounded in 3 Granola calls + live QA org read), and the
> visual-direction brief (design-scout).

---

## 0. FORBIDDEN — absent from every concept (Tyler's law)

- NO dark mode, NO `prefers-color-scheme`, NO theme toggle anywhere. Light mode is the only mode.
- NO extraction/overall confidence, NO % confidence, NO confidence rings/gauges/meters.
  (The record carries `Overall_Confidence__c=73.5`, `Market_Routing_Confidence__c=Medium`,
  `Requires_Review__c=true` — DO NOT render any of it.)
- NO reprocessing controls / scope checkboxes / panels / "reprocess" buttons.
- NO tabs / tabset. NO timeline / story layout. NO split-ledger.
- NO fabricated completeness/quality judgments ("fleet history looks incomplete" — banned).
  Represent only what is ACTUALLY PRODUCED / present. Use the word **PRODUCED**, never "received".

---

## 1. HIERARCHY THESIS (what makes it beautiful)

Ruthless hierarchy + air + restraint + one accent color. **Summary First, Detail Later** —
the hero band must be scannable in **under 2 seconds**. ONE hero fact → a tier of **4–6**
primary figures max → quiet supporting rows. Whitespace (not nested boxes) is the grouping
mechanic. Avoid boxes-inside-boxes.

---

## 2. TYPE LADDER (system fonts only — LWS-legal, no @font-face/CDN)

Font stack (all concepts):
`-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif`

Only these 6 steps. Never more than 3 weights on screen (400 / 600 / 700).

| Role | Size | Weight | Notes |
|---|---|---|---|
| Hero figure (the single biggest fact) | 2.5–3rem (40–48px) | 700 | `letter-spacing:-0.02em`, color `#181818` |
| KPI value (primary tier) | 1.5–1.75rem (24–28px) | 700 | `font-variant-numeric: tabular-nums` (money must align) |
| Section / card title | 1.375rem | 700 | `#181818` |
| Eyebrow / KPI label | 0.75rem | 600 | UPPERCASE, `letter-spacing:0.05em`, `#706e6b` — the premium tell |
| Body / value | 0.875rem | 400–500 | line-height 1.5 |
| Micro / footnote / source | 0.75rem | 400 | `#706e6b`, line-height 1.4 |

Money/counts always `tabular-nums`, right-aligned in columns. Large numerals get the -0.02em tighten.

---

## 3. SPACING (8px grid — the "expensive" feel)

Every gap/pad is a multiple of 8 (allow 4 & 12): `4, 8, 12, 16, 24, 32, 48`.

- Outer card padding: **24–32px** (be generous — round 1 was cramped).
- KPI-to-KPI gap: **32px** min.
- Label-to-value vertical gap: **4px** (label hugs its value).
- KPI tier → secondary rows: **24–32px** + a hairline divider.
- Section internal row rhythm: 12–16px.

Map to SLDS spacing hooks with rem fallbacks in the real LWC: `var(--lwc-spacing-*, 1.5rem)`.

---

## 4. PALETTE (Salesforce blue + graded neutrals, LIGHT ONLY)

Use these as CSS custom properties. Blue is used SPARINGLY (accent rail, one active state, links,
the single most important marker) — not as a background for everything.

```
--brand:        #0176d3;   /* Salesforce blue — accent only */
--brand-dark:   #032d60;   /* deep blue for large headings if desired */
--ink:          #181818;   /* primary text */
--text-2:       #514f4d;   /* secondary text */
--muted:        #706e6b;   /* labels / meta */
--border:       #e5e5e5;   /* hairline (SLDS neutral: #dddbda ok) */
--surface:      #ffffff;   /* card surface */
--recess:       #fafaf9;   /* recessed/grouping surface (warm gray) */
--blue-tint:    #e8f4fd;   /* accent chip fill */  --blue-tint-bd: #c9e5f9;

/* STATUS (color + icon + TEXT always — never color alone) */
--produced:     #2e7d32;   --produced-bg: #edf7ed;   /* GREEN */
--partial:      #a86403;   --partial-bg:  #fff8e1;    /* AMBER (text darkened for AA) */
--notprod:      #ba0517;   --notprod-bg:  #fef0f0;   --notprod-bd: #f5c0c0;  /* RED */
--na:           #706e6b;   --na-bg:       #f3f3f3;    /* GREY neutral */
```

Elevation: soft shadow `0 1px 3px rgba(0,0,0,0.08)` (SLDS card shadow ok). Radius: **8px** outer
cards (modern), **4px** inner elements.

---

## 5. COMPONENT VOCABULARY (shared motifs → concepts feel like one family)

- **Left accent rail** (THE Astrus signature): 3–4px colored `border-left` + tinted fill +
  `border-radius: 0 8px 8px 0`. Use it as the connective system across the summary.
- **Eyebrow label + big value** pairing (§2) for every KPI/stat.
- **Status token** = colored dot/icon + one-word label + the LOB's key figure inline. Never color alone.
- **Persistent LEGEND** wherever statuses appear — defines each color in plain words (see §7).
- **Definition grid** (dl/dt/dd) for facts — hairline row separators, NOT bordered boxes.
- **One honest micro-viz max per concept** (pure CSS/inline SVG): a slim stacked bar for exposure mix
  or per-state split, 6–8px tall. NO donuts/gauges/rings. NO faked time series/sparklines.
- **Quiet data table** for schedules: shaded header `#fafaf9`, tabular-num right-aligned money,
  12–16px rows, hairline separators, zero vertical gridlines.

LWC build target (note at top of each HTML file): HTML template + modern JS + SLDS/SLDS2 CSS +
base `lightning-*` components (`-card, -badge, -icon, -formatted-number/-currency/-date-time,
-datatable, -pill, -layout`). Inline SVG/CSS only. Self-contained, LWS-safe: no external
frameworks/CDNs/fonts, no eval, no remote innerHTML. Accessibility: contrast ≥4.5:1, semantic
HTML (headings, dl, th scope), status = color+icon+text.

---

## 6. RANKED PRIORITIES — what the summary shows, in EMPHASIS ORDER

Grounded in Rich ("did we get GL present? are the exposures there. Auto present… at-a-glance
thing at the top"; "boom, boom, boom"), Josh (STANDARDIZED, not free-form), Karen (per-LOB,
expectation-driven), Tyler (standardized; PRODUCED / NOT PRODUCED).

- **P0 — WHO (the anchor, biggest text):** Named insured / applicant. + # of named insureds,
  a one-line description of ops, New vs Renewal.
- **P1 — LINES OF BUSINESS PRODUCED (the centerpiece, "boom boom boom"):** each LOB with a crisp
  PRODUCED status (§7). Most visual weight after the header.
- **P2 — KEY EXPOSURE INSIDE each LOB (not a separate abstract block):** WC→payroll, GL→revenue,
  Commercial Auto→vehicle count. The exposure value IS the proof the LOB was produced.
- **P3 — COMMERCIAL AUTO VEHICLE COUNT ("did I get 47 vehicles"):** show the NUMBER as a
  first-class fact. Show the count, NOT a completeness verdict.
- **P4 — LOSS RUNS (the highest-value MISSING flag):** Produced vs Not produced. Rich most wants
  this in 2 seconds. Render as a **submission-level chip**, NOT an LOB row. When present, show
  valuation date / years.
- **P5 — SUPPORTING CONTEXT (compact, secondary):** effective date / term, state(s), submitting
  broker, carrier/market, TPA.

Every concept must make P0 the biggest, P1 the dominant zone, and make P4 (loss-run "Not produced")
visually pop. P5 stays quiet.

---

## 7. PER-LOB "PRODUCED" STATUS SEMANTICS (crisp, derivable-only, self-evident)

Frame (state this in the legend, verbatim-ish): *the LOB is requested on the submission; the
question is whether Astrus PRODUCED data for it.* Three states ONLY. Every status shows
**color + icon + text label**, and a persistent legend defines each in words.

| State | Color | Icon | Label | MEANS (legend text) | Derivable from |
|---|---|---|---|---|---|
| **Produced** | GREEN `#2e7d32` | check | "Produced" | LOB present AND its expected exposure was produced (WC has payroll, GL has revenue, Auto has a vehicle schedule with N units). | exposure field populated / child records exist |
| **Partial** | AMBER `#a86403` | warning | "Partial" | LOB present and produced, but a specific expected element is verifiably empty (e.g. no effective date; Auto with 0 vehicles). | point to the actual empty field — never a vibe |
| **Not produced** | RED `#ba0517` | error | "Not produced" | LOB is on the submission but NO data was produced for it (no exposure, no schedule). = "requested but nothing came back." | exposure/schedule all null |

Rules: (1) word is **PRODUCED / NOT PRODUCED**, never "received". (2) Every status must map to a
real field; if you can't derive it, show the data value itself, not a status. (3) **Loss run is a
submission-level chip (Produced / Not produced), NOT an LOB row.** (4) Do not invent completeness.

---

## 8. CANONICAL DATA — A&B Painting SUB 022667

Live from `alliantinsurance--qa`, Id `a1bO300000DN1NbIAL`, read 2026-07-13. Use these EXACT values
in ALL 6 concepts.

**Identity / header**
- Submission #: **SUB 022667**  ·  Status: **Clearance Requested**
- Named insured (primary): **A & B Painting, Inc.**
- Named insureds (3, all "Artisan Contractor"): A & B Painting, Inc.; A & B Painting West, Inc.;
  ACR Commercial Drywall, Inc.
- New/Renewal: **New**
- Description of ops: *"Painting and wall-covering services for commercial and public projects;
  works as a subcontractor under competitive-bid/fixed-price contracts; greater Northern
  California; union workforce."*
- Avg employment: **50**

**Term / context**
- Effective **07/01/2026** · Expiration **07/01/2027** (12-month term)
- State(s): **CA** (primary); **HI** (Excess + HI Auto)
- Submitting broker (text): **R2 Solutions**
- Carrier / Market: **Chubb** · Market action: **Place** · TPA: **ESIS**
- Locations (2): 672 Walsh Ave, Santa Clara, CA 95050; 2270 Palou Ave, San Francisco, CA 94124

**Lines of business — 5 produced (`Number_of_LOB__c = 5`)**

| # | LOB | State | Key exposure | Status |
|---|---|---|---|---|
| 1 | Workers' Compensation | CA | Payroll **$4,928,350** | GREEN — Produced |
| 2 | General Liability | CA | Revenue **$8,500,000** (subcontracted $1,050,000: wall covering, specialty coatings, scaffolding) | GREEN — Produced |
| 3 | Commercial Auto | CA | **31 vehicles** | GREEN — Produced |
| 4 | Commercial Auto | HI | **2 vehicles** (HI is a SEPARATE LOB — Hawaii needs its own policy) | GREEN — Produced |
| 5 | Excess Liability | CA + HI | no effective date on record (`Effective_Date__c` null) | AMBER — Partial |

**Headline exposures**
- WC payroll: **$4,928,350**
- GL revenue: **$8,500,000**
- Commercial Auto vehicle schedule: **33 vehicles total** (31 CA + 2 HI)

**Loss runs: NOT PRODUCED** — submission loss fields null (`Loss_Run_Valuation_Date__c=null`,
`Loss_Run_Days_Old__c=null`). Render as submission-level chip: **"Loss runs — Not produced."**
This is THE red/"missing" flag Rich wants surfaced.

**DO-NOT-SHOW (in the record, forbidden):** Overall_Confidence__c 73.5; Market_Routing_Confidence__c
Medium; Requires_Review__c true.

Secondary sample (mention only; all concepts render 022667): **J.H. Lynch SUB 022651** available.

---

## 9. THE 6 CONCEPTS (fixed slugs → builders)

All beautiful, all highly informative, all pure visual, all evolving from §6 priorities. Distinct
takes on the SAME prioritized facts. At least one (concept 01) is the strong evolution of Tyler's
favorite per-LOB status. No two concepts may share the same primary layout metaphor.

| Slug | Name | Builder | One-line thesis |
|---|---|---|---|
| `concepts/01-coverage-status-board.html` | Coverage Status Board | builder-1 | The evolved per-LOB "boom boom boom" — each LOB a status card (Produced/Partial/Not produced) with its exposure figure baked in, a plain-words legend, and the loss-run flag. |
| `concepts/02-executive-hero.html` | Executive Hero | builder-2 | One cinematic hero: applicant name huge, a 4–6 KPI strip, a compact LOB status rail, and the loss-run "Not produced" flag that pops. |
| `concepts/03-exposure-first.html` | Exposure-First | builder-3 | Lead with the money: headline exposures as the biggest figures with one honest stacked exposure-mix micro-viz, LOB status attached to each figure. |
| `concepts/04-underwriter-datasheet.html` | Underwriter Datasheet | builder-4 | The "listed brief" made genuinely beautiful: a disciplined definition-grid of facts + a quiet vehicle-schedule table, elegance from type/spacing/alignment restraint. |
| `concepts/05-priority-signal-cards.html` | Priority Signal Cards | builder-5 | The underwriter's ranked checklist as a curated set of signal cards in emphasis order (WHO → LOBs → exposures → loss runs → context), each card one clear signal. |
| `concepts/06-coverage-map.html` | Coverage Map | builder-6 | A spatial overview: a state × line-of-business coverage matrix showing what's produced where (CA/HI), with exposures and the loss-run flag around it. |

Each concept renders §8 data exactly, reflects §6 emphasis order, uses §7 status semantics with a
legend, and obeys §0 forbiddens. Top-of-file HTML comment must note the LWC mapping.
