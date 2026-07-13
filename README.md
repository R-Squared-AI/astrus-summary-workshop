# Astrus Underwriter-Controlled Reprocessing Solution Workshop

**At-a-Glance Submission Summary — LWC design workshop**

This repository hosts a live gallery of six distinct, production-realistic design concepts
for the **at-a-glance submission summary**: a single high-signal panel that sits at the top of
an Astrus submission record in Salesforce Lightning and is the first thing an underwriter sees.

Each concept answers an underwriter's opening question in seconds — *what came in, what's solid,
what's missing* — and, where a gap exists, surfaces **underwriter-controlled reprocessing** right
where the data is missing or low-confidence.

## View it live

**Gallery:** https://r-squared-ai.github.io/astrus-summary-workshop/

Open the landing page, then open any concept card. Each mockup is self-contained and renders the
same sample submission so the six designs can be compared head-to-head. A light/dark toggle is in
the top-right of every page.

## The shared sample submission

Every concept renders the same record so they are directly comparable:

- **A & B Painting, Inc. — SUB 022667** (real `alliantinsurance--qa` record)
- New business · Target market **Chubb** · Submitting broker **R2 Solutions**
- Status: **Clearance Requested** (3rd of 4: Pre-processed → Extraction Completed → **Clearance Requested** → Cleared)
- Overall confidence **73.5%** (amber) · Effective 07/01/2026 → 07/01/2027 · 3 named insureds (unverified)
- LOBs received: **WC, GL, Commercial Auto**
- Exposures: WC payroll **$4,928,350** · GL revenue **$8,500,000** · Commercial Auto vehicle schedule **PARTIAL**
- **Loss run MISSING** — the star gap (Josh's exact "they didn't have a loss run I need" example)
- A new file **`Loss_Run_2021-2025.pdf`** just landed on the communication record (notification bell = 1),
  driving the reprocess story: *flag → new file arrived → pick per-LOB scope → confirm → reprocess*
- GL Proposed Bound Premium **$42,500 is locked** — reprocess never overwrites a populated bound premium

## The six concepts

| # | Concept | Pole | Thesis |
|---|---------|------|--------|
| 01 | **Command Center** | Standalone panel | Every decision-driving number on one dense, glanceable board — scan in 2 seconds. |
| 02 | **Underwriter Brief** | Standalone panel | A standardized, deterministic one-block-per-topic briefing that reads like a senior underwriter's file note (same skeleton every time, not free-text LLM). |
| 03 | **Triage Traffic-Light** | Field-evidence extension | A standardized red/amber/green present-vs-missing checklist of the top things underwriters need, with reprocess right where the gap is. |
| 04 | **Tabbed Deep-Dive** | Standalone panel | A permanent at-a-glance header band, then every dimension one click away — never leave the summary to drill in. |
| 05 | **Timeline / Story** | Standalone panel | The submission as a lifecycle pipeline with reprocess at the stage that stalled. |
| 06 | **Split Ledger** | Field-evidence extension | A two-column ledger — extracted value vs source & confidence — with inline edit and targeted reprocess per field. |

### Tab-vs-panel spread (a deliberate decision aid)

Josh & Rich left the tab-vs-panel question open, so the six concepts deliberately span both poles:

- **Standalone panel / tab pole** — 01, 02, 04, 05 — a dedicated summary component above the record.
- **Field-evidence extension pole** — 03 & 06 — extend the existing data-quality / field-evidence UI.

## LWC feasibility

These are **static HTML/CSS/JS mockups** of a future Lightning Web Component, not the component
itself (a real LWC needs an org to render). They are hand-rolled to match SLDS tokens, the type
ladder, and Astrus confidence bands (≥90 green · 70–89 amber · <70 red), with **no external
frameworks or CDNs**. Every pattern maps 1:1 to a buildable LWC:

- **Can use:** HTML templates (`lwc:if`/`for:each`), modern ES getters + `Intl.NumberFormat`,
  SLDS/SLDS2 styling hooks, base components (`lightning-card`, `-badge`, `-icon`, `-formatted-number`,
  `-progress-ring`, `-progress-indicator`, `-datatable`, `-tabset`, `-button`), Lightning Data Service
  (`getRecord`/`getFieldValue`) + imperative Apex `@wire`, inline SVG/CSS charts.
- **Avoided:** third-party JS frameworks/CDNs, `eval`, untrusted `innerHTML`, direct
  `window`/`document`/`fetch` (LWS-sandboxed), SOQL/DML in loops.

Each concept page carries its own 1–2 sentence LWC-feasibility note naming the base components / LDS
wires the real LWC would use.

## Repo layout

```
index.html              # the gallery landing page
concepts/
  01-command-center.html
  02-underwriter-brief.html
  03-triage-traffic-light.html
  04-tabbed-deep-dive.html
  05-timeline-story.html
  06-split-ledger.html
SHARED-SPEC.md          # shared visual standard + canonical submission + reprocessing decisions
```

## Scope & notes

- **Scope:** only the at-a-glance summary component. Not a pipeline-diagnosis or data-fix exercise.
- **Input gap (noted, non-blocking):** the workshop was grounded in the shared visual spec, the
  canonical SUB 022667 record, and the underwriter priorities from the Josh & Rich call. A Google
  Doc referenced during the engagement was not available to this build; concepts proceed on the
  Granola/underwriter-priority context and Salesforce record inspection.
