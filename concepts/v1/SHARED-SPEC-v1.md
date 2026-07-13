# SHARED SPEC — Astrus "At-a-Glance Submission Summary" Workshop

**ALL 6 concept mockups MUST render the SAME submission and share the SAME visual language.**
Read this file before building. Your queen (`design-lead`) sends you your 2 concept specs separately.

---

## 0. WHAT WE ARE BUILDING (context)

A single high-signal panel that sits at the TOP of an Astrus Submission record in Salesforce
Lightning — the first thing an underwriter sees. Josh Eliason's ask (verbatim intent): *"if I'm
an underwriter, I want to see what came in"* and in *"two seconds"* know e.g. *"they didn't have
a loss run that I need."* Format must be **standardized, deterministic, PRESENT/MISSING** ("super
simple, even checkboxes"), identical every time. This is an **underwriter-controlled reprocessing**
engagement — every concept must surface where data is missing/low-confidence AND how the
underwriter triggers a targeted reprocess.

**Tab-vs-panel axis (Josh/Rich left this open, want us to span it):**
- Concepts **01 Command Center, 02 Brief, 04 Tabbed, 05 Timeline** = standalone panel/tab pole.
- Concepts **03 Triage, 06 Split Ledger** = "top-line-of-field-evidence" pole (extend the existing
  quality/field-evidence shape). Note this positioning in each mockup's footer note.

---

## 1. SHARED VISUAL STANDARD (SLDS-like — use verbatim across all 6)

**Do NOT load external CDNs/fonts.** Hand-roll SLDS-matching tokens inline. Self-contained HTML.

### Fonts
System stack approximating Salesforce Sans:
`font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;`

### Color tokens (define as CSS custom properties on `:root`, map light+dark)
| Token | Light | Meaning |
|---|---|---|
| `--brand` | `#0176d3` | Salesforce blue — structure/brand/primary action ONLY |
| `--brand-dark` | `#032d60` | Navy — headers, titles |
| `--tint-1` | `#f3f8fe` | lightest panel tint |
| `--tint-2` | `#eaf2fc` | card tint |
| `--tint-3` | `#dbeafe` | group-label background |
| `--success` | `#2e7d32` | green — RECEIVED / present / conf ≥90 |
| `--warning` | `#f57f17` | amber — PARTIAL / unverified / conf 70–89 |
| `--error`   | `#b71c1c` | red — MISSING / conf <70 / needs review |
| `--neutral-text` | `#181818` | body |
| `--neutral-muted`| `#5c5c5c` | labels/captions |
| `--border` | `#e5e5e5` | card borders |
| `--surface` | `#ffffff` | card background |

**Color = meaning, never decoration.** Blue is brand/structure. Green/amber/red ONLY for
status/severity. NEVER color-alone: every severity chip = colored dot/icon + TEXT label.

### Dark mode (`@media (prefers-color-scheme: dark)` + `:root[data-theme="dark"]`)
Surface `#1b1b1b`, panel tint `#22252a`, text `#f3f3f3`, muted `#b0b0b0`, border `#3a3a3a`.
Keep semantic greens/ambers/reds but lighten ~12% for contrast (`#4caf50`/`#ffb300`/`#ef5350`).
Provide a light/dark toggle button in each mockup (top-right) that stamps `data-theme` on `:root`.

### Confidence bands (LIVE Astrus thresholds — use everywhere)
`≥90` green · `70–89` amber · `<70` red.

### Type ladder (from live astrus1 code — reuse verbatim)
- Card title: `1.375rem / 700`, navy (`--brand-dark`)
- Group label: `1.125rem / 700`, navy on `--tint-3`, 4px left border `--brand`
- Object label: `1rem / 700`
- Sub / caption: `0.875rem / 600`, muted
- Big stat number: `2rem–2.5rem / 700` (largest type on any tile — numbers scanned first)

### Layout / spacing
SLDS spacing rhythm: 0.5rem / 0.75rem / 1rem / 1.5rem. Cards = `--surface`, `1px solid --border`,
`border-radius: 0.25rem`, subtle shadow `0 2px 4px rgba(0,0,0,.08)`, generous padding (`1rem`).
Responsive: 12-col fluid grid, collapse to 1 col under ~640px. Right-align all currency/numbers.

### Charts (inline SVG or pure CSS only — NO libraries; this is 1:1 with real LWC)
Confidence = SVG donut/progress-ring. Exposure mix = CSS flex bars. Loss trend = tiny inline SVG
polyline sparkline. Completeness = segmented bar. Every chart needs an `aria-label` + text equivalent.

### Icons
Use inline SVG or simple unicode/emoji-free glyphs approximating SLDS domain icons. Consistent
icon-per-domain (Insured=person, Coverage/LOB=shield, Exposure=chart, Loss=bolt/incident,
Broker=briefcase, Quality=shield-check, Docs=file, Reprocess=refresh/⟳).

### Accessibility
WCAG 2.1 AA. Semantic headings, `aria-label` on charts/rings, text label on every severity chip,
contrast ≥4.5:1, keyboard-focusable buttons/tabs.

### Chrome (make it feel like a real Lightning record page)
Wrap each mockup in a faux Lightning record-page frame: a slim top breadcrumb strip
("Astrus Submission › SUB 022667"), then THE COMPONENT is the star. Add a small footer note card:
**one-line thesis**, **target underwriter job**, **tab-vs-panel positioning**, and a
**"Why intuitive / LWC-feasibility"** blurb (2–3 sentences). Include the light/dark toggle.

---

## 2. THE CANONICAL SHARED SUBMISSION — render THIS in all 6

**A & B Painting, Inc. — SUB 022667** (real QA record; derived signals below are the illustrative
present/partial/missing states — use them identically across all 6 so the gallery is comparable).

### Identity
- Submission #: **SUB 022667**
- Named Insured (primary): **A & B Painting, Inc.**
- Additional named insureds: **3 total** · Verified: **No** (amber flag)
- New/Renewal: **New**
- Effective: **2026-07-01** → Expiration: **2027-07-01** (12-mo term)
- Domicile State: **— (not captured)** (minor amber "missing" chip — optional to show)
- Submitting Broker: **R2 Solutions**
- Target market / carrier: **Chubb**
- AI Processed: **Yes**

### Status (real Status__c path — 4 stages; current = 3rd)
`Pre-processed` → `Extraction Completed` → **`Clearance Requested`** (CURRENT) → `Cleared`
AI-engine file counts: Files in submission **6**, Preprocessed **6**, Extracted **6**.

### Lines of Business (model at the TYPE level — Josh's mental model)
| LOB | Status | Detail |
|---|---|---|
| Workers' Compensation | **RECEIVED** (green) | 1 line |
| General Liability | **RECEIVED** (green) | 1 line |
| Commercial Auto | **RECEIVED** (green) | 2 quote lines |
(Total LOB records: 5. No expected-but-missing LOB for this submission — the MISSING hero-demo is
the loss run, below. If a concept needs to show a missing-LOB state, show it as a greyed
"Umbrella — not received" row labeled optional.)

### Exposures (per LOB)
| LOB | Exposure | Value | Signal |
|---|---|---|---|
| WC | Projected Payroll | **$4,928,350** | present (green) |
| GL | Projected Revenue | **$8,500,000** | present (green) |
| Commercial Auto | Vehicle / Power-Unit schedule | **PARTIAL** | amber — fleet history incomplete (computed present/partial/missing signal; there is no single vehicle-count field) |

### Loss History  ← the star "MISSING" demo
- Loss runs: **MISSING** (red). `Loss_Run_Valuation_Date = null`. Caption: "No loss run received —
  underwriter flagged this is needed." (This is Josh's exact anti-example.)

### Data Quality / Confidence
- **Overall Confidence: 73.5%** (amber band)
- Requires Review: **Yes**
- Named Insureds Verified: **No** (amber)
- Reason codes (field-evidence): "Loss run not found" (red) · "Vehicle schedule partial" (amber) ·
  "Named insureds unverified" (amber) · "Domicile state not captured" (amber, optional)

### Reprocessing surface (locked design decisions D1–D6)
- **Files on the communication record** (Astrus_Submissions_Communication__c):
  `ACORD_125_Application.pdf` · `ACORD_130_WC.pdf` · `ACORD_137.pdf` · `Vehicle_Schedule.xlsx` ·
  `Property_SOV.pdf` · `Broker_Cover_Email.msg`
- **NEW file just landed** (notification bell = **1**): `Loss_Run_2021-2025.pdf` — this directly
  answers the missing-loss-run flag. Every concept should tell this story: *flag → new file
  arrived → pick scope → reprocess.*
- **Per-LOB reprocess scope** = checkboxes: ☐ Workers' Compensation ☐ General Liability
  ☑ Commercial Auto (pre-checked because the new loss run + partial vehicle schedule affect it).
  Reprocess updates ONLY checked LOBs; preserves manual edits elsewhere (D5).
- **Locked field (D6):** GL **Proposed Bound Premium = $42,500 🔒** — reprocess NEVER overwrites a
  populated Proposed Bound Premium. Show a lock glyph + tooltip "Locked — reprocess won't overwrite."
- **Manual "Reprocess selected files" button** with a **confirm step** (D2 — no silent reprocess).
- **Unlink, not delete (D4):** file rows have an "Unlink" (recoverable) action, not a destructive
  delete. Note admins-only for permanent delete.
- Reprocess control lives in a **separate, lower-emphasis action zone** (card footer or dedicated
  Actions tile), echoing the existing `astrusSubmissionUpdateReplay` LWC — never mixed into read data.

### Secondary submission (for optional "submission switcher" flourish only; NOT required)
J.H. Lynch & Sons — SUB 022651: conf 77.2%, Payroll **$0 (known WC-drop bug → red needs-review)**,
GL Revenue $230,000,000, 18 named insureds, RI, broker "The Driscoll Agency". Use only if a concept
wants a switcher; the canonical render is always A&B SUB 022667.

---

## 3. LWC FEASIBILITY GUARDRAILS (keep every concept buildable)
CAN: HTML templates (`lwc:if`/`for:each`), modern ES (getters for derived state, `Intl.NumberFormat`),
SLDS + SLDS2 styling hooks (`--slds-g-color-*`), base components (lightning-card/-badge/-icon/
-formatted-number/-progress-ring/-progress-indicator/-datatable/-tabset/-button), Lightning Data
Service (`getRecord`/`getFieldValue`) + imperative Apex `@wire`, inline SVG/CSS charts.
AVOID: 3rd-party JS frameworks/CDNs (React/D3/Chart.js/jQuery), inline `<script src>`, `eval`,
`innerHTML` of untrusted markup, direct `window`/`document`/`fetch` (LWS-sandboxed), SOQL/DML in loops.
NO TypeScript. Everything in these mockups maps 1:1 to a real LWC.

Each mockup's footer must include a 1–2 sentence **LWC-feasibility note** naming the base components
/ LDS wires the real LWC would use.
