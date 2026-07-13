# Astrus Submission Summary — Design Round 2

**At-a-glance submission summary — LWC design workshop**

This repository hosts a live gallery of six new, production-realistic design concepts for the
**at-a-glance submission summary**: a single high-signal panel pinned to the top of an Astrus
submission record in Salesforce Lightning and the first thing an underwriter sees. These are
**pure visual mockups** — a beautiful, highly informative snapshot of *what the submission
contains and which lines of business were produced*.

## View it live

**Gallery:** https://r-squared-ai.github.io/astrus-summary-workshop/

Open the landing page, then open any concept card. Each mockup is self-contained and renders the
same sample submission so the six designs can be compared head-to-head. **Light mode only** —
nothing in Salesforce is dark mode.

## Round 2 — what changed

Round 1 feedback reset the bar. Round 2 is governed by these rules (see `SHARED-SPEC-V2.md`):

- **Light mode only.** No dark mode, no theme toggle, no `prefers-color-scheme` anywhere.
- **No extraction confidence.** Confidence % means nothing to underwriters — removed entirely.
- **No reprocessing controls.** Reprocessing is out of scope for the summary — removed entirely.
- **No tabs, no timeline/story, no split-ledger** layouts.
- **Represent only what is actually PRODUCED.** No fabricated completeness/quality judgments. The
  word is **PRODUCED / NOT PRODUCED**, never "received".
- **Keep & evolve the favorite:** the per-line-of-business status ("boom, boom, boom"), now with
  crisp, self-evident semantics and a persistent plain-words legend.

## Per-LOB "PRODUCED" status semantics

Each line of business is requested on the submission; the question is whether Astrus **produced**
data for it. Three states only, always shown as color + icon + text, defined by a persistent legend:

| State | Color | Means |
|---|---|---|
| **Produced** | green | LOB present and its expected exposure was produced (WC has payroll, GL has revenue, Auto has a vehicle schedule). |
| **Partial** | amber | LOB produced, but a specific expected element is verifiably empty (e.g. no effective date). |
| **Not produced** | red | LOB is on the submission but no data was produced for it — requested, but nothing came back. |

Loss runs are surfaced as a **submission-level flag** (Produced / Not produced), not an LOB row.

## The shared sample submission

Every concept renders the same record so they are directly comparable:

- **A & B Painting, Inc. — SUB 022667** (real `alliantinsurance--qa` record, read 2026-07-13)
- New business · 3 named insureds (Artisan Contractor) · Submitting broker **R2 Solutions** · Carrier **Chubb** · TPA **ESIS**
- Term **07/01/2026 → 07/01/2027** · States **CA** (primary) + **HI** (Excess + HI Auto)
- **5 lines of business produced:**
  - Workers' Compensation (CA) — payroll **$4,928,350** — Produced
  - General Liability (CA) — revenue **$8,500,000** — Produced
  - Commercial Auto (CA) — **31 vehicles** — Produced
  - Commercial Auto (HI) — **2 vehicles** — Produced (HI is a separate LOB)
  - Excess Liability (CA + HI) — no effective date on record — **Partial**
- Headline exposures: WC payroll **$4,928,350** · GL revenue **$8,500,000** · Commercial Auto **33 vehicles total**
- **Loss runs — Not produced** (submission loss fields null) — the key "missing" flag underwriters want in two seconds

## The six concepts

| # | Concept | Thesis |
|---|---------|--------|
| 01 | **Coverage Status Board** | The evolved per-LOB "boom boom boom" — each LOB a status card (Produced/Partial/Not produced) with its exposure figure baked in, a plain-words legend, and the loss-run flag. |
| 02 | **Executive Hero** | One cinematic hero: applicant name huge, a 4–6 KPI strip, a compact LOB status rail, and the loss-run "Not produced" flag that pops. |
| 03 | **Exposure-First** | Lead with the money: headline exposures as the biggest figures with one honest stacked exposure-mix micro-viz, LOB status attached to each figure. |
| 04 | **Underwriter Datasheet** | The listed brief made genuinely beautiful: a disciplined definition-grid of facts plus a quiet vehicle-schedule table — elegance from restraint. |
| 05 | **Priority Signal Cards** | The underwriter's ranked checklist as curated signal cards in emphasis order (who → LOBs → exposures → loss runs → context). |
| 06 | **Coverage Map** | A spatial state × line-of-business coverage matrix showing what's produced where (CA / HI), with exposures and the loss-run flag. |

## LWC feasibility

These are **static HTML/CSS mockups** of a future Lightning Web Component, not the component
itself. They are hand-rolled to match SLDS tokens, the type ladder, and the status semantics, with
**no external frameworks, fonts, or CDNs** (LWS-safe). Every pattern maps to a buildable LWC:
HTML templates (`lwc:if`/`for:each`), modern ES getters + `Intl.NumberFormat`, SLDS/SLDS2 styling
hooks, base components (`lightning-card`, `-badge`, `-icon`, `-formatted-number`/`-currency`/
`-date-time`, `-datatable`, `-pill`, `-layout`), Lightning Data Service (`getRecord`) + imperative
Apex `@wire`, and inline SVG/CSS charts. Each concept page carries its own top-of-file LWC-mapping
note. Light mode only.

## Repo layout

```
index.html                          # the gallery landing page (light mode)
concepts/
  01-coverage-status-board.html
  02-executive-hero.html
  03-exposure-first.html
  04-underwriter-datasheet.html
  05-priority-signal-cards.html
  06-coverage-map.html
  v1/                               # Round 1 — superseded by feedback (archived)
    01-command-center.html
    02-underwriter-brief.html
    03-triage-traffic-light.html
    04-tabbed-deep-dive.html
    05-timeline-story.html
    06-split-ledger.html
    SHARED-SPEC-v1.md
SHARED-SPEC-V2.md                   # the Round-2 visual standard + canonical submission
```

## Scope & notes

- **Scope:** only the at-a-glance summary component. Pure visual mockups.
- Sample data reflects the live `alliantinsurance--qa` record SUB 022667.
