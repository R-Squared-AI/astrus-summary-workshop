# Astrus Submission Summary — Design Round 3

**At-a-glance submission summary — LWC design workshop**

This repository hosts a live gallery of four converged design concepts for the **at-a-glance
submission summary**: a single high-signal card pinned to the top of an Astrus submission record
in Salesforce Lightning and the first thing an underwriter sees. These are **pure visual
mockups** — a beautiful, highly informative snapshot that answers, in one glance, *which lines of
business are on the submission and what was produced vs. missing* across the two things
underwriters care about most: **exposures** and **loss runs**.

## View it live

**Gallery:** https://r-squared-ai.github.io/astrus-summary-workshop/

Open the landing page, then open any concept card. Each mockup is self-contained and renders the
same sample submission so the four designs can be compared head-to-head. **Light mode only** —
nothing in Salesforce is dark mode.

## Round 3 — what changed

Rounds 1 and 2 reset the bar. Round 3 converges on **one direction** (see `SHARED-SPEC-V3.md`):

- **One cohesive card, not a dashboard or a stack of cards.** Everything lives in a single tile.
- **Hierarchy communicates priority.** The layout mirrors how much underwriters care about each
  thing — an instant present/missing scan, not a ranked-importance chart.
- **Organizing axis = line of business.** Under each LOB: a present / count / total scan for both
  **exposure** and **loss runs** — not a single headline number.
- **Exposures and loss history are elevated** and first-class — the "good stuff."
- **Right-sized gradient LOB tiles** are the centerpiece (kept from the favorite round-2 hero, but
  no longer oversized).
- **Light mode only.** No dark mode, no theme toggle, no `prefers-color-scheme`.
- **No extraction confidence / %, no reprocessing controls, no tabs.**
- **No gratuitous charts and no filler** (dropped: Term, "number of lines of business", the
  vehicle micro-bar).
- **Represent only what is actually PRODUCED.** No fabricated completeness/quality judgments. The
  word is **PRODUCED / NOT PRODUCED**, never "received".

## Per-LOB "PRODUCED" status semantics

Each line of business is requested on the submission; the question is whether Astrus **produced**
data for it. Three states only, always shown as color + icon + text, defined by a persistent legend:

| State | Color | Means |
|---|---|---|
| **Produced** | green | Line present and its expected item was produced (WC payroll, GL revenue, an Auto power-unit count, or a loss run). |
| **Partial** | amber | Produced, but a specific expected field is verifiably empty (e.g. no effective date). |
| **Not produced** | red | The item is on the submission but no data was produced for it — requested, but nothing came back. |

Loss runs are surfaced **per line of business** (present / count / total) *and* rolled up as a
first-class standalone element (here: **Not produced**, expected from TPA ESIS) — never an
afterthought, never a fabricated zero.

## The shared sample submission

Every concept renders the same record so they are directly comparable:

- **A & B Painting, Inc. — SUB 022667** (real `alliantinsurance--qa` record, read 2026-07-13)
- New business · 3 named insureds (Artisan Contractor) · Submitting broker **R2 Solutions** · Carrier **Chubb** · TPA **ESIS**
- States **CA** (primary) + **HI** (Excess + HI Auto)
- **5 lines of business produced:**
  - Workers' Compensation (CA) — payroll **$4,928,350** — Produced
  - General Liability (CA) — revenue **$8,500,000** (+$1,050,000 subcontracted) — Produced
  - Commercial Auto (CA) — **31 power units** — Produced
  - Commercial Auto (HI) — **2 power units** — Produced (HI is a separate LOB)
  - Excess Liability (CA + HI) — no effective date on record — **Partial**
- **Loss runs — Not produced** on every line (submission loss fields null) — the key "missing"
  flag underwriters want in two seconds.

## The four concepts

| # | Concept | Thesis |
|---|---------|--------|
| 01 | **LOB Tile Grid** | Gradient LOB tiles as the hero — a tidy right-sized grid, each tile a per-LOB present/count/total scan for exposure and loss runs, with the loss-history gap band prominent alongside. |
| 02 | **Priority Ranked** | One card flowing top-to-bottom in the underwriter's scan order (LOBs + exposures → loss runs → quiet context) with visual weight tapering. |
| 03 | **Exposure Led** | Leads with the money (headline exposures up top), then resolves into the gradient LOB tiles + a first-class loss-history band. One card, not a dashboard. |
| 04 | **Compact Executive** | The refined best-of round-2's Executive Hero — filler removed (fleet bar / Term / LOB-count), gradient tiles kept + right-sized, loss history added as first-class. |

## LWC feasibility

These are **static HTML/CSS mockups** of a future Lightning Web Component, not the component
itself. They are hand-rolled to match SLDS tokens, the type ladder, and the status semantics, with
**no external frameworks, fonts, or CDNs** (LWS-safe). Every pattern maps to a buildable LWC:
HTML templates (`lwc:if` / `for:each`, so the LOB grid is expandable to 3/5/7+ lines), modern ES
getters + `Intl.NumberFormat`, SLDS/SLDS2 styling hooks, base components (`lightning-card`,
`-badge`, `-icon`, `-formatted-number` / `-currency` / `-date-time`), Lightning Data Service
(`getRecord`) + imperative Apex `@wire`, and inline SVG/CSS. **CSS `linear-gradient()` on the LOB
tiles is confirmed feasible** — it is plain, LWS-safe, scoped-stylesheet CSS with zero external
assets, and ports verbatim into the component `.css`. Each concept page carries its own
top-of-file LWC-mapping note. Light mode only.

## Repo layout

```
index.html                          # the gallery landing page (light mode)
SHARED-SPEC-V3.md                   # the Round-3 visual standard + canonical submission
concepts/
  01-lob-tile-grid.html             # Round 3 — featured
  02-priority-ranked.html
  03-exposure-led.html
  04-compact-executive.html
  v2/                               # Round 2 — superseded (archived)
    01-coverage-status-board.html
    02-executive-hero.html
    03-exposure-first.html
    04-underwriter-datasheet.html
    05-priority-signal-cards.html
    06-coverage-map.html
    SHARED-SPEC-V2.md
  v1/                               # Round 1 — superseded (archived)
    01-command-center.html
    02-underwriter-brief.html
    03-triage-traffic-light.html
    04-tabbed-deep-dive.html
    05-timeline-story.html
    06-split-ledger.html
    SHARED-SPEC-v1.md
```

## Scope & notes

- **Scope:** only the at-a-glance summary component. Pure visual mockups.
- Sample data reflects the live `alliantinsurance--qa` record SUB 022667.
