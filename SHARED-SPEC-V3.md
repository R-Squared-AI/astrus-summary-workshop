# SHARED SPEC — ROUND 3 (the law for all 4 concepts)

**Astrus Submission Summary** — a beautiful, state-of-the-art, world-class, HIGHLY
INFORMATIVE, PURE-VISUAL "at-a-glance" summary pinned to the TOP of an Astrus insurance
submission in Salesforce. Buildable as **ONE Lightning Web Component**. **LIGHT MODE ONLY.**
All 4 concepts render the SAME submission (**A&B Painting SUB 022667**) so they compare cleanly.

> Supersedes round-1 `SHARED-SPEC.md` and round-2 `SHARED-SPEC-V2.md`.
> Sources: `mission.v3` (Tyler's cumulative feedback = THE GOLDEN STANDARD), the round-3
> transcript-cited priority ranking (`priorities-analyst`), and the confirmed-good DNA from
> round-2 concept 02 "Executive Hero."

---

## 0. THE GOLDEN STANDARD — restated as a CHECKLIST (every G# is law; the reviewer grades each)

**Purpose**
- [ ] **G1** — Beautiful, state-of-the-art, world-class, highly-informative, PURE-VISUAL summary for the TOP of a submission; one Salesforce LWC.
- [ ] **G2** — **IT IS ONE TILE / ONE CARD.** Not a dashboard. Not a stack of separate cards. Not multi-panel sprawl. Everything lives in a single cohesive card.
- [ ] **G3 (REFRAMED by Tyler — read carefully)** — This is **NOT** a ranked/weighted "importance chart." It is a **QUICK AT-A-GLANCE SUMMARY of the high-priority things underwriters care about**, so that in ONE glance — BEFORE digging into the submission — the underwriter instantly sees the STATE: *"looks like we're missing exposures, missing loss runs, have GL, have Auto"* — boom boom boom. **Instant situational awareness of what's PRESENT vs MISSING.** Use the priority ranking (§6) to DECIDE WHICH items to surface at a glance — NOT to build a weighted-hierarchy visual. The **PRODUCED / NOT-PRODUCED (present vs missing) status across the key items IS the core value** — make MISSING things pop instantly (missing loss runs, any missing/partial exposure) so gaps register in seconds without reading. **Favor SIMPLICITY and instant clarity over density.** It's a fast pre-read, not a deep dashboard. (Stored: hive `mission.v3.refinement`.)

**Grounding**
- [ ] **G4** — Priority ranking is intelligently + exhaustively extracted from the Granola transcripts (Rich, Josh, underwriters) and cited. (See §6.)
- [ ] **G5** — Content centers on what Rich & Josh emphasized: **LINES OF BUSINESS, their EXPOSURES, and LOSS HISTORY / loss runs.** Exposures + loss history are the "good stuff" — **ELEVATE them.**

**Hard bans**
- [ ] **G6** — NO dark mode / theme toggle / `prefers-color-scheme`. Light mode only.
- [ ] **G7** — NO extraction confidence or % anywhere.
- [ ] **G8** — NO reprocessing controls of any kind.
- [ ] **G9** — NO tabs / tabset.
- [ ] **G10** — NO gratuitous graphs/charts/bars that don't carry real meaning. (The bar under "Auto Vehicles 33" is banned — 31 CA + 2 HI needs no bar.) A viz is allowed ONLY if it genuinely communicates something an underwriter needs.
- [ ] **G11** — NO filler / low-value metrics. DROP: **Term** (~always 1 yr, not decision-relevant) and **"number of lines of business."** Cut anything not decision-relevant.
- [ ] **G12** — NEVER fabricate completeness/quality verdicts. Show real values/counts (e.g. vehicle COUNT), never an invented "complete/incomplete" judgment.

**Confirmed-good DNA — build on this**
- [ ] **G13** — **LINE-OF-BUSINESS TILES are the centerpiece** and the thing Tyler likes — specifically round-2 Executive Hero's **GRADIENT LOB tiles** ("I definitely like these lines of business tiles down here where there's the gradient"). Each tile: LOB + status + its key EXPOSURE. **Keep the gradient. TILES MUST BE TASTEFULLY SIZED — NOT too big** (round-2 concept 01 was rejected for oversized tiles).
- [ ] **G14** — Per-LOB status: defined semantics + plain-words legend; word is **"PRODUCED"**, never "received"; RED = requested but nothing produced; AMBER = a specific expected field verifiably empty; GREEN = produced.
- [ ] **G15** — **LOSS HISTORY / loss-run status is a FIRST-CLASS, prominent element** — not an afterthought.

**Visual**
- [ ] **G16** — Genuinely beautiful, refined, restrained, professional; Salesforce-native but best-in-class. Right-sized, not cluttered, NOT a dashboard. (Round 1 > round 2 — do not regress.)
- [ ] **G17** — LWC-feasible: HTML + modern JS + SLDS/SLDS2 CSS + base Lightning components, light mode, fully self-contained (inline CSS/SVG, no external deps). **CSS gradients ARE feasible in LWC** — confirm this in each file's LWC-mapping comment.

---

## 0.5 EXPLICIT REMOVE LIST (must be ABSENT from every concept)

| Removed | Why (G#) |
|---|---|
| Dark mode / theme toggle / `prefers-color-scheme` | G6 |
| Any extraction/overall confidence, %, confidence rings/gauges/meters | G7 (record carries `Overall_Confidence__c=73.5`, `Market_Routing_Confidence__c=Medium`, `Requires_Review__c=true` — render NONE of it) |
| Reprocess buttons / scope checkboxes / reprocessing panels | G8 |
| Tabs / tabset | G9 |
| The **fleet/vehicle bar** under "Auto Vehicles 33" (meaningless viz) | G10 |
| Any donut / gauge / ring / faked sparkline / time series | G10 |
| **Term** KPI (12-month term) | G11 |
| **"Number of lines of business"** as a headline KPI | G11 |
| Fabricated "complete / incomplete / looks thin" verdicts | G12 |
| The word "received" (use PRODUCED) | G14 |

---

## 1. THE ONE-CARD LAYOUT SYSTEM (this is the convergence — read twice)

Everything is **ONE `<lightning-card>`** — a single surface with internal REGIONS separated by
**whitespace and hairline dividers**, never by nested boxes-in-boxes and never by detached
sub-cards. The regions flow top-to-bottom in **priority order** (§6). Read it as one object.

- **Single surface.** White card (`--surface`), 8px radius, one soft shadow, one hairline border.
  Internal regions are delineated by generous padding + 1px `--border` dividers or bare whitespace.
- **NOT a dashboard, NOT a card-stack.** No grid of equal peer cards. No panel walls. If a region
  looks like it could be lifted out and stand alone as its own widget, it's wrong — fold it back in.
- **Right-sized.** Outer padding 24–32px. The whole thing should feel like a premium record header,
  not a page. Fits at the top of a Salesforce record without scrolling to comprehend the essentials.
- **One accent color** (Salesforce blue) used sparingly. Status color (green/amber/red) is the only
  other color, and only on status elements.

### Type ladder (system fonts only — LWS-legal, no @font-face/CDN)
Stack: `-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif`.
Max 3 weights on screen (400 / 600 / 700). Money & counts always `font-variant-numeric: tabular-nums`.

| Role | Size | Weight | Notes |
|---|---|---|---|
| Hero figure (single biggest fact) | 2.5–3rem | 700 | `letter-spacing:-0.02em`, `#181818` |
| Primary value (exposure/loss) | 1.5–1.75rem | 700 | tabular-nums, money aligned |
| Region / card title | 1.375rem | 700 | `#181818` |
| Eyebrow / label | 0.75rem | 600 | UPPERCASE, `letter-spacing:0.05em`, `#706e6b` — the premium tell |
| Body / value | 0.875rem | 400–500 | line-height 1.5 |
| Micro / footnote | 0.75rem | 400 | `#706e6b` |

### Spacing (8px grid — the "expensive" feel)
Multiples of 8 (allow 4 & 12): `4, 8, 12, 16, 24, 32, 48`. Outer card pad 24–32px; label→value 4px;
region → region 24–32px + a hairline; row rhythm 12–16px. Whitespace is the grouping mechanic.

---

## 2. PALETTE (Salesforce blue + graded neutrals, LIGHT ONLY)

```
--brand:        #0176d3;   /* Salesforce blue — accent only, used sparingly */
--brand-dark:   #032d60;   /* deep blue for large headings if desired */
--ink:          #181818;   /* primary text */
--text-2:       #514f4d;   /* secondary text */
--muted:        #706e6b;   /* labels / meta */
--border:       #e5e5e5;   /* hairline (SLDS neutral #dddbda also ok) */
--surface:      #ffffff;   /* the ONE card surface */
--recess:       #fafaf9;   /* recessed grouping surface (warm gray) */
--blue-tint:    #e8f4fd;   --blue-tint-bd: #c9e5f9;   /* accent chip fill */

/* STATUS (color + icon + TEXT always — never color alone) */
--produced:     #2e7d32;   --produced-bg: #edf7ed;   --produced-bd: #cde8ce;   /* GREEN */
--partial:      #a86403;   --partial-bg:  #fff8e1;    --partial-bd:  #f3e2b0;   /* AMBER (AA-darkened) */
--notprod:      #ba0517;   --notprod-bg:  #fef0f0;    --notprod-bd:  #f5c0c0;   /* RED */
--na:           #706e6b;   --na-bg:       #f3f3f3;                              /* GREY neutral */
```
Elevation: `0 1px 3px rgba(0,0,0,0.08)` (hero card may use `0 4px 16px rgba(0,0,0,.06), 0 1px 3px rgba(0,0,0,.08)`).
Radius: **8px** outer card, **4–6px** inner elements.

---

## 3. THE CENTERPIECE DNA — RIGHT-SIZED GRADIENT LOB TILES (G13, non-negotiable)

The confirmed-good element Tyler explicitly likes. Evolved from round-2 Executive Hero's bottom rail.

- **Gradient fill:** each tile carries a subtle horizontal status gradient fading to white, e.g.
  `background: linear-gradient(90deg, var(--produced-bg) 0%, #ffffff 62%);` (amber/red variants).
  **This is the "gradient" Tyler praised. Keep it.** Confirm in the LWC comment that CSS
  `linear-gradient` is fully supported in LWC (it is — plain CSS, no external dep).
- **Left accent rail:** 4px `border-left` in the status color, `border-radius: 0 6px 6px 0`.
- **RIGHT-SIZED — not too big.** Compact tiles: ~48–72px tall, comfortable but dense. Round-2
  concept 01 was rejected for oversized tiles. Think a tidy row/grid of tiles, not big billboards.
- **Each tile contains, at a glance:** (1) LOB name + state chip(s); (2) status token (dot/icon +
  the word Produced/Partial/Not produced — never color alone); (3) **its key EXPOSURE figure baked
  in** (the exposure IS the proof the LOB was produced — WC→payroll, GL→revenue, Auto→vehicle count).
- **Present/missing legibility (Tyler's G3 reframe):** a PRESENT exposure is a loud confident number;
  a MISSING/empty exposure must read instantly as a HOLE (muted "—" + a visible "exposure missing"
  flag on that tile), never a quiet blank the eye skips. Absence is a first-class signal.

### 3.1 THE PER-LOB "ITEM SCAN" MODEL (Tyler correction #2 — this is the tile data model)

Exposures are **NOT** a single decorative headline number. **Line of business is the organizing
axis**, and under each LOB the tile answers a **PRESENCE + COUNT + TOTAL** scan for BOTH exposures
and loss runs:

- **EXPOSURE per LOB → present? · count · total.** Show ONLY figures that actually exist — **never
  fabricate a count** (G12). Honest per-type semantics for A&B:
  | LOB | present? | count | total |
  |---|---|---|---|
  | Workers' Comp (CA) | YES | *(no honest sub-count in the record — omit, show total only)* | payroll **$4,928,350** |
  | General Liability (CA) | YES | *(no count — omit)* | revenue **$8,500,000** (breakdown: $1,050,000 subcontracted) |
  | Commercial Auto (CA) | YES | **31** power units | 31 units |
  | Commercial Auto (HI) | YES | **2** power units | 2 units |
  | Excess Liability (CA+HI) | PARTIAL | — | no exposure figure; AMBER driven by missing effective date |
  Where a type has a natural count (Auto = power units), show count. Where it doesn't (WC payroll,
  GL revenue), show the TOTAL only and DO NOT invent a count. Total auto across the two LOBs = 33.
- **LOSS RUNS per LOB (and rolled up to the SOURCE = TPA) → present? · count · total.** When produced
  elsewhere, the honest, real fields are: **# loss-run years** of history (~7–8 yrs), **# claims**,
  **total incurred** (and/or paid) + valuation date — e.g. "N claims · $X incurred · Y yrs."
  **For A&B all loss fields are NULL → NOT PRODUCED** on every LOB and at the account level.
  **CRITICAL (G12): NULL is ABSENCE, not zero — render "Loss runs — not produced," NEVER "0 claims /
  $0 incurred"** (that would fabricate a produced-but-empty result). Roll-up framing = the real source
  dimension we have: **"expected from TPA ESIS."** Do NOT manufacture per-named-insured splits (3 NIs,
  but no per-NI loss data). See §5.
- **DO-NOT-INVENT list (builders, memorize):** WC → no count (payroll total only); GL → no count
  (revenue total + subcontracted breakdown only); Auto → no dollar total (power-unit count + CA/HI
  breakdown only); Excess → no exposure number (AMBER + "missing effective date"); Loss runs on A&B →
  NULL rendered as "not produced," never 0/$0. The ONLY real breakdowns to show: Auto CA 31 / HI 2,
  GL subcontracted $1.05M, Excess missing-field = effective date, loss-run source = TPA ESIS.
- **EXPANDABLE (Tyler):** "start with the top three, or these seven, and add more later." Build the
  tile/row as a **repeatable component** that scales to 3, 5, 7+ LOBs and more exposure types — do
  NOT hardcode. (For A&B all 5 rows fit; the affordance matters for bigger accounts. No truncation here.)

> Note on Commercial Auto (analyst double-count guard): the CA/HI split is legitimate (HI is a real
> org carve-out — HI Auto handled as its own LOB). You MAY show it as TWO tiles (CA 31 / HI 2) OR as a
> SINGLE Commercial Auto tile with a "CA 31 · HI 2" chip — both are honest; pick whichever keeps YOUR
> card cleaner (G16). **The only real figures are CA 31, HI 2, and roll-up 33** — invent nothing more:
> NO per-state dollar totals, NO per-state loss-run counts, NO per-state effective dates. **Avoid the
> double-count read: never present 31 + 2 + 33 as three peers** — if you show 33, label it clearly as
> **"33 total (CA 31 / HI 2)"** roll-up context, not a third quantity. Loss runs on BOTH CA and HI =
> "not produced" (NULL), same as every LOB.
- Tiles may be a vertical rail OR a responsive 2–3 col grid depending on the concept's hierarchy
  treatment — but they are always the visual centerpiece of the single card.

---

## 4. PER-LOB "PRODUCED" STATUS SEMANTICS + 3-STATE LEGEND (G14)

Frame (state in the legend, verbatim-ish): *the LOB is requested on the submission; the question is
whether Astrus PRODUCED data for it.* Three states only. Every status = **color + icon + text**, and
a **persistent plain-words legend** defines each.

| State | Color | Icon | Label | MEANS (legend text) | Derivable from |
|---|---|---|---|---|---|
| **Produced** | GREEN `#2e7d32` | check | "Produced" | Line present AND its expected exposure was produced (WC payroll, GL revenue, an Auto vehicle schedule with N units). | exposure field populated / child records exist |
| **Partial** | AMBER `#a86403` | warning | "Partial" | Produced, but a specific expected element is verifiably empty (e.g. no effective date). Point to the ACTUAL empty field — never a vibe. | the named empty field |
| **Not produced** | RED `#ba0517` | error | "Not produced" | Line is on the submission but NO data was produced — "requested, nothing came back." | exposure/schedule all null |

Rules: word is **PRODUCED / NOT PRODUCED**, never "received." Every status maps to a real field; if
you can't derive it, show the data value, not a status. Never invent completeness (G12).

---

## 5. LOSS HISTORY — FIRST-CLASS, PROMINENT (G15)

Loss runs / loss history are among the "good stuff" (G5) and Tyler wants them ELEVATED — a first-class
region, NOT a small chip tucked at the bottom. For A&B: **loss runs NOT produced** (submission loss
fields null). Treat this as a prominent, high-contrast element:
- Its own clearly-visible region with the RED "Not produced" treatment (icon + word + one plain
  sentence: "No loss history was produced — no valuation date, no claims data on record.").
- Placed HIGH in the hierarchy per §6 (near the exposures, not in the footer).
- When loss history IS produced (other submissions), this region shows the PRESENCE + COUNT + TOTAL:
  valuation date + # loss-run years + # claims + total incurred. Design it to gracefully hold real
  loss data, not just the empty state (expandable, per §3.1).
- **Axis (Tyler correction #2):** loss-run presence is tracked per LOB AND may roll up by **named
  insured / TPA** (A&B's TPA = ESIS; Mercury is another example). Reflect the roll-up where relevant —
  e.g. a prominent submission/TPA-level "Loss runs — Not produced" headline, and (optionally) a
  per-LOB present/absent tick so the eye sees the gap is total. Do NOT invent claim counts (G12).

---

## 6. PRIORITY-DRIVEN HIERARCHY (G3 + G4) — the transcript-cited ranking → the PRESENT/MISSING scan

**FINALIZED from `priorities-analyst` (full ranking in hive `priorities.v3`), reconciled with
Tyler's G3 reframe.** The North Star (why the summary exists): it answers ONE underwriter question
in ~2 seconds — *"Is this submission worth carrying forward, and do I have enough to quote?"* Rich's
own verbatim mental model: *"did we get GL present? Are the exposures there? Auto present?… oh yeah,
they didn't have a loss run that I need."*

**THE DESIGN IS AN INSTANT THREE-BEAT PRESENT/MISSING SCAN — NOT a weighted bar chart of importance.**
Build it so the eye clocks, in order and in ~2 seconds:

> **BEAT 1** — Which EXPECTED lines of business were PRODUCED? (green/amber/red per LOB)
> **BEAT 2** — Does each LOB have its EXPOSURE, or is it a hole? (the loud number on each tile)
> **BEAT 3** — Did we get the LOSS RUNS I need — yes or no? (prominent standalone signal)

**Rule of thumb:** the ranking DECIDES WHICH items are on the card and how loud each *class* of item
is — it does NOT mean literally rank-ordering everything by font size. A **MISSING** item must be as
visually prominent as a present one (absence is a first-class signal). Simplicity + instant clarity
over density (Tyler).

| Tier | Element | Treatment | Grounding (cited) |
|---|---|---|---|
| **1 · #1** | **LINES OF BUSINESS + per-LOB status** — the gradient LOB tiles CENTERPIECE. Each expected LOB reads green (produced) / amber (a field empty) / red (nothing produced). | The hero grid/rail of the card; most weight. Right-sized tiles (G13). | Rich "did I get this line of business"; UAT per-LOB green model (Josh) |
| **1 · #2** | **EXPOSURES — the headline number ON each LOB tile** (WC→payroll, GL→revenue, Auto→power units/vehicles). ELEVATE: co-equal with the LOB name. A MISSING exposure must read instantly as a HOLE, not a quiet blank. | Big value inside each tile; the "good stuff." | Rich "are the exposures there"; Josh Jul-7 exposure mechanics; Karen owns format |
| **2 · #3** | **LOSS HISTORY / LOSS RUNS — produced vs NOT produced.** THE purest presence/absence signal (Josh: "we don't store that" — non-derivable, so it's pure signal of what arrived). For A&B = prominent **RED "Loss runs — not produced."** | First-class standalone element, co-loud with the LOB grid, near the top of the eye path. NOT buried, NOT a tiny footer chip. | Rich (most emotional/frequent): "they didn't have a loss run that I need"; Josh non-derivable point |
| **3 · #4** | **Named insured(s)** — who is the risk (A&B = 3). Orientation, not the decision. | Compact header. Quiet. | training: UWs anchor on identity first |
| **3 · #5** | **Broker · Carrier · TPA** (R2 Solutions · Chubb · ESIS) | Small metadata chips/line. Quiet. | clearance/routing context |
| **3 · #6** | **Geography / state** (CA + HI) — as a state chip PER LOB (Auto: CA 31 / HI 2), never a bar. | Inline chips only. Quiet. | "special state exposures" AI-insights category |

**DROP entirely (not ranked):** Term (~1 yr, G11); "# lines of business" (G11); extraction confidence/%
(G7); gratuitous bars incl. the "Auto Vehicles 33" bar (G10); fabricated complete/incomplete verdicts
(G12); effective date as a standalone hero KPI (fold it into the Excess-Liability AMBER status driver only).

**Every concept must** make Tier-1 (LOB tiles + their exposures) the dominant zone, make loss history
(Tier-2) pop as a co-loud standalone gap signal, keep Tier-3 quiet, and make any MISSING/PARTIAL item
jump out at a glance.

---

## 7. CANONICAL DATA — A&B Painting SUB 022667 (use EXACT values in all 4 concepts)

Live from `alliantinsurance--qa`, Id `a1bO300000DN1NbIAL`, read 2026-07-13.

**Identity / header (P0)**
- Submission #: **SUB 022667** · Status: **Clearance Requested**
- Named insured (primary): **A & B Painting, Inc.**
- Named insureds (3, all "Artisan Contractor"): A & B Painting, Inc.; A & B Painting West, Inc.; ACR Commercial Drywall, Inc.
- New/Renewal: **New**
- Description of ops: *"Painting and wall-covering services for commercial and public projects; works as a subcontractor under competitive-bid/fixed-price contracts; greater Northern California; union workforce."*
- Avg employment: **50**

**Context (P4 — quiet)**
- Effective **07/01/2026** · Expiration **07/01/2027**  *(term is CONTEXT only — do NOT show a "Term" KPI, G11)*
- State(s): **CA** (primary); **HI** (Excess + HI Auto)
- Submitting broker (text): **R2 Solutions**
- Carrier / Market: **Chubb** · Market action: **Place** · TPA: **ESIS**
- Locations (2): 672 Walsh Ave, Santa Clara, CA 95050; 2270 Palou Ave, San Francisco, CA 94124

**Lines of business — 5 (`Number_of_LOB__c = 5`, but do NOT show the count as a KPI, G11)**

| # | LOB | State | Key exposure (baked into its tile) | Status |
|---|---|---|---|---|
| 1 | Workers' Compensation | CA | Payroll **$4,928,350** | GREEN — Produced |
| 2 | General Liability | CA | Revenue **$8,500,000** (subcontracted $1,050,000: wall covering, specialty coatings, scaffolding) | GREEN — Produced |
| 3 | Commercial Auto | CA | **31 vehicles** | GREEN — Produced |
| 4 | Commercial Auto | HI | **2 vehicles** (HI is a SEPARATE LOB — Hawaii needs its own policy) | GREEN — Produced |
| 5 | Excess Liability | CA + HI | no effective date on record (`Effective_Date__c` null) | AMBER — Partial |

**Headline exposures (P3)**
- WC payroll: **$4,928,350**  ·  GL revenue: **$8,500,000**  ·  Commercial Auto: **33 vehicles total (31 CA + 2 HI)** — show the COUNT, never a bar (G10).

**Loss runs (P2): NOT PRODUCED** — submission loss fields null (`Loss_Run_Valuation_Date__c=null`,
`Loss_Run_Days_Old__c=null`). Prominent RED "Loss runs — Not produced" region. THE missing flag Rich wants.

**DO-NOT-SHOW (in the record, forbidden — G7):** Overall_Confidence__c 73.5; Market_Routing_Confidence__c Medium; Requires_Review__c true.

Secondary sample (mention only; all concepts render 022667): **J.H. Lynch SUB 022651** available.

---

## 8. THE 4 CONCEPTS — TIGHT VARIATIONS ON ONE DIRECTION (fixed slugs → builders)

**All four are unmistakably ONE beautiful single card**, gradient LOB tiles as centerpiece, exposures
+ loss history elevated, hierarchy = priority (§6), zero filler/gratuitous charts (§0.5). They differ
ONLY in LAYOUT / HIERARCHY treatment of that same content — NOT in metaphor. No dashboards, no card-stacks.

| Slug | Name | Builder | One-line thesis |
|---|---|---|---|
| `concepts/01-lob-tile-grid.html` | LOB Tile Grid | builder-1 | The right-sized gradient LOB tiles ARE the hero of the card (refined Executive-Hero DNA) — a tidy grid/rail of tiles with exposure baked into each, loss-history band prominent right beside/below them. |
| `concepts/02-priority-ranked.html` | Priority Ranked | builder-2 | One card, elements stacked in STRICT §6 priority order with clear visual-weight tapering (hierarchy literally IS priority); gradient LOB tiles as the core zone. |
| `concepts/03-exposure-led.html` | Exposure Led | builder-3 | Same single card, LEADS with the top exposure figures (P3 pulled up as big primary values) then the gradient LOB tiles + loss history — cohesive, NOT a dashboard. |
| `concepts/04-compact-executive.html` | Compact Executive | builder-4 | The tight, dense-but-elegant "best of Executive Hero" — filler removed (no fleetbar, Term, LOB-count), loss history ADDED as first-class, everything condensed into one refined card. |

**Each concept file:** renders §7 data exactly; reflects §6 emphasis order; uses §4 status semantics
with the 3-state legend; obeys §0/§0.5 bans; centerpiece = §3 right-sized gradient LOB tiles; loss
history = §5 first-class. **Top-of-file HTML comment MUST note the LWC mapping incl. explicit
confirmation that CSS gradients are LWC-feasible** (G17).

---

## 9. LWC-FEASIBILITY (state at top of every file)

Component `c-astrus-submission-summary` pinned to top of the Submission record page.
`@wire(getRecord)` for header/context fields + Apex `@wire getLinesOfBusiness()` returning per-LOB
`{ name, state, statusToken, exposureLabel, exposureValue }`; status derived in Apex per §4.
Markup: one `<lightning-card>`; `<lightning-formatted-currency/-number/-date-time>` for money/counts/
dates; status tokens = `<lightning-icon>` (utility:check / warning / error) + text; loss region =
`<lightning-icon icon-name="utility:error">` + text. **CSS `linear-gradient` on the LOB tiles is
plain CSS and fully supported inside an LWC's scoped stylesheet — no external dependency (G17).**
Styling: SLDS/SLDS2 tokens via `var(--lwc-spacing-*, …)` with rem fallbacks. Light mode only.
LWS-safe: system fonts, inline CSS/SVG, no CDN/@font-face/eval/remote innerHTML. A11y: semantic
headings, `<dl>` for facts, status = color+icon+text, contrast ≥ 4.5:1.
