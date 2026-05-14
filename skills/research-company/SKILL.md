---
name: research-company
description: |
  Design-time meta-skill. Given a company name or web domain, profile
  the organisation against the public web and emit a structured
  `org-brief` YAML conforming to `brief.schema.yaml` v2. The brief
  is the single input to the (future) `compose-org` skill that forks
  the `zava-control-plane` substrate into a customer-flavoured demo
  (e.g. `zava-control-plane-colt`).

  DESIGN INTENT: the brief must be detailed enough to drive a
  **digital-clone-grade** fork — breadth parity with Zava's own
  envelope (≥25 vertical-flavoured domains, 50–80 personae across a
  5–6-tier org chart, 12–18 vertical entity kinds, 12–20 third-party
  systems, 8–20 cadenced rituals, 4–8 named narrative arcs).

  Procedure is fixed. Every factual claim carries a `confidence`
  discriminator and references one or more entries in `sources[]`.
  Gaps go into `uncertainties[]` — never invented.
audience: design-time-only
forbidden-runtime: true
---

# research-company

You are the operator of a thirteen-phase research procedure that
profiles a real-world organisation against the public web and emits
a structured org-brief YAML. You follow this skill literally. When
uncertain, you record an entry in `uncertainties[]` and continue —
you do **not** guess.

> **Forbidden.** This skill writes to **one** path:
> `specs/<slug>-org-brief.yaml` (relative to the `zava-design-skills`
> repo root, where `<slug>` is the kebab-case form of the company
> name). It MUST NOT edit anything under `../zava-control-plane/`
> or any other location.

## Inputs

You are invoked one of two ways:

1. **With a company name or web domain.** "Run `research-company` on
   `colt.net`." Pick a `slug` (kebab-case, ≤ 16 chars) and an output
   path `specs/<slug>-org-brief.yaml`. Read it back to the operator
   before continuing if the slug is ambiguous.

2. **With an existing in-progress brief.** Resume at the lowest
   unfilled phase.

## Tooling

You require these tools and must use them, in this priority order, for
every factual claim:

1. `web_fetch` against the company's **own** properties (`/about`,
   `/leadership`, `/investors`, the latest annual report or
   capital-markets-day PDF, `/press` / `/newsroom`, `/sustainability`,
   `/governance`). Self-reported data carries `confidence: medium`
   unless cross-corroborated.
2. `web_search` for cross-corroboration. Two independent secondary
   sources concurring lifts `confidence` to `high`.
3. `web_fetch` against authoritative third parties: Wikipedia
   (factual scaffolding), Companies House / SEC EDGAR / equivalent
   national registry, regulator filings, theofficialboard.com,
   theorg.com, equilar.com, news of record (FT/Reuters/Bloomberg/
   sector trades).
4. `web_search` for stack signal: company jobs board ("we use
   Salesforce, ServiceNow…"), vendor case-study pages, Gartner /
   Forrester / Omdia write-ups.
5. `web_search` for organisational depth: LinkedIn company search
   "VP at X", "Director of Y at X", trade-press leadership profiles,
   business-school case studies. **This is the phase that gets you
   from 11 ELT rows to 60+ personae.**

Never rely on a single answer-engine summary; every claim must point
at one or more rows in `sources[]`.

## Schema

The output conforms to [`brief.schema.yaml`](brief.schema.yaml).
Read it once before you begin. Key rules:

- Every `Fact` requires `{value, confidence, source_refs?, notes?}`.
- Every `Source` row needs `{id, url, accessed, kind, used_for}`.
- Every truly-missing field is recorded in `uncertainties[]`.
- `meta.status` walks `in_progress` → `needs_review` → `ready`.

## The thirteen phases

### Phase 0 — Bootstrap (one-shot)

Create the spec stub at `specs/<slug>-org-brief.yaml` with skeleton
keys + empty arrays.  All subsequent phases append via `edit`.

### Phase A — Identity & description

Wikipedia + the company's own `/about`. Fill
`identity.{name, short_name, slug, domain, description, brand_voice,
industry, sub_industry, tagline}`.

### Phase B — Ownership & size

SEC EDGAR / Companies House / annual report. Fill
`ownership.{structure, parent, ticker, founded, key_shareholders}`
and `size.{employees, revenue_usd, customers_count, assets_count}`.

`size.assets_count` is the **vertical-native scale signal**: on-net
buildings for telco; aircraft in fleet for airline; dealers for auto
OEM; branches for retail bank; plants for FMCG. Don't skip — this is
what the demo dashboards anchor on.

### Phase C — Geography

`geo.{hq, regions, countries_present, key_hubs, footprint_notes}`.
`footprint_notes[]` collects vertical-native scale callouts
("250+ cloud PoPs", "1,300 dealers", "180 routes flown").

### Phase D — Subsidiaries & legal entities

Walk Companies House (or equivalent). Cap at 15 — holding plus the
materially-sized opcos. Each gets `{id, name, country, role,
regulator, confidence}`.

### Phase E — Functions

Distil 10–12 canonical functions. ≤ 3 hero. Cover at minimum:
finance, hr, legal-and-regulatory, sales, the operational hero
function for the vertical (network-ops / flight-ops / manufacturing
/ delivery), customer-success, tech, and (since 2024 norms)
security.

### Phase F — Org chart depth (the big phase)

**This is the phase that gets the brief from "ELT summary" to
"digital clone."** Produce 50–80 PersonaNode rows in
`org_chart[]` covering EVERY function. For each function:

- the function head (level 2 — already on `/leadership`)
- the SVPs / Directors that report to them (level 3)
- one or two regional or sub-team leads (level 4–5)
- 1–3 individual-contributor archetypes with `count` set (level 6)

Sources:
- `/leadership` for level 2.
- LinkedIn search "<title> at <company>" for level 3-5: "Director of
  Network Operations at Colt", "VP Finance at Colt", "Head of
  Wholesale Sales at Colt".
- Annual report management-team list.
- theofficialboard.com / theorg.com for free-text reports.
- Trade-press leadership interviews give sub-team structure cues.

Use `count: N` for "N of these exist" so you don't have to invent
60 distinct names — compose-org generates them via faker later.
**Real names go on the ELT (level 1–2) only**; below that, `name`
stays empty and the persona is "the role" not "the person."

Validation: every function in `functions[]` has ≥3 personae rows;
hero functions have ≥6.

### Phase G — Vertical entity kinds

12–18 rows in `vertical_entity_kinds[]`. Cover the agency-table
swaps (Brand/Campaign/Pitch/MediaPlan) plus any net-new kinds the
vertical needs. Reference heuristic table:

| Vertical | Likely set |
|---|---|
| **Telco** | Service / Circuit / Quote / CapacityPlan / Site / PointOfPresence / Customer / Incident / Ticket / NetworkElement / IPPrefix / BGPPeering / CrossConnect / Licence / Spectrum |
| **Airline** | Route / Sector / Slot / Roster / Aircraft / Crew / Pairing / Booking / Bay / Gate / MROOrder / Spare / FuelHedge / Licence / FrequentFlyer |
| **Auto OEM** | Model / Launch / DealerOrder / Plant / SKU / DealerNetwork / Vehicle / Recall / ProductionRun / TierOneSupplier / Spec / Homologation |
| **FMCG** | Brand / Campaign / Listing / ChannelPlan / SKU / Plant / Recipe / RawMaterial / DistributorCenter / Promotion |
| **Bank** | Product / Cohort / Mandate / TreasuryBook / Counterparty / Branch / Mortgage / Card / Position / Limit / Account |
| **Retail** | Product / Range / Promotion / FloorPlan / Store / SKU / Supplier / Distribution / Shrinkage / Cohort |

Adapt to the actual company. Every entry needs `relations[]`
showing how it joins the graph (e.g. `Circuit TERMINATES_AT Site`).

### Phase H — Proposed domains (the breadth phase)

**Produce 25–35 rows in `proposed_domains[]`** spanning every
function. ≤ 3 hero. Avoid duplicating the generic Zava domains
already in the substrate; only list vertical-specific workflows.

Method:
1. List the org's functions.
2. For each function, brainstorm 2–5 workflows specific to the
   vertical at that function. (Telco / network-ops: incident-to-
   restore, capacity-augment, planned-maintenance-window, fibre-cut-
   recovery, NOC-shift-handover, spectrum-allocation, …)
3. Add 3–5 cross-function meta-workflows ("lumen-integration-cutover"
   touches tech + customer-success + network-ops).
4. Mark each with `cadence:` (ad-hoc / daily / weekly / monthly /
   quarterly / annual) — drives the simulator spawn rate.

Validation: every function has ≥2 domain rows; hero functions ≥4.

### Phase I — Stack

12–20 rows in `stack.systems[]`. Three angles:

1. **Vendor case-study pages** (Salesforce/ServiceNow/SAP/Amdocs/
   Cisco/Microsoft all publish logo walls) — `confidence: high` when
   the company logo appears.
2. **Company jobs board** — `site:careers.<domain> Salesforce` etc.
   `confidence: medium`.
3. **Industry-standard inference** — list the role and 2–3
   `candidates[]` with `confidence: inferred`.

Always include: CRM, ITSM, ERP, HCM, identity (likely Entra ID at
Microsoft-heavy customers), esign, observability, and the
vertical-native operational system (OSS/BSS for telco; reservations
for airline; MES + PLM for OEM; OMS for retail; core-banking for
bank).

### Phase J — Regulators

One row per country in `geo.countries_present` × material regulatory
domain. Add horizontal regulators (data-protection, anti-trust) for
the HQ jurisdiction. Confidence is `high` for any regulator that
covers a country listed in `geo.countries_present`.

### Phase K — Cadenced rituals

12–20 rows in `cadenced_rituals[]`. Cover at least one ritual per
function. Examples: daily NOC shift handover; weekly capacity
review; monthly RFO close; quarterly regulator filing; annual
licence renewal; annual budget cycle; quarterly board pack; weekly
sales pipeline scrub. Each row gets `cadence:`.

### Phase L — Narrative arcs & KPIs

4–8 named `narrative_arcs[]` drawn from the last 24 months of press
+ analyst coverage. Each gets a `headline`, a `summary`, the
`function_focus[]` it touches, and a `timeline_hint`.

Then 6–12 `kpi_cinematics[]` rows — the HUD KPIs the vertical
demands. Telco: MTTR, on-time-provisioning, capacity-utilisation %,
churn, NPS, network-availability %, cost-per-bit. Airline: OTP%,
load-factor, ASK, RASK, cancellation-rate, MRO-cycle-time.

### Phase M — Cross-check & finalise

Pick **five random `confidence: high` claims**. Re-run `web_search`
and confirm. Downgrade or flag any that don't recheck.

Validate the brief against `brief.schema.yaml`:

- All `minItems` constraints satisfied (`org_chart[] ≥ 40`,
  `proposed_domains[] ≥ 20`, `vertical_entity_kinds[] ≥ 8`,
  `stack.systems[] ≥ 10`, `cadenced_rituals[] ≥ 8`,
  `narrative_arcs[] ≥ 3`, `kpi_cinematics[] ≥ 6`).
- Every Fact has `confidence`.
- Every `source_refs[]` entry exists as an id in `sources[]`.
- `meta.status` flips to `needs_review`.

Print a short summary:
- counts: subsidiaries N, personae N, proposed_domains N,
  cadenced_rituals N, narrative_arcs N, sources N
- 5 highest-confidence claims
- 5 most material uncertainties
- the one-line "compose-org" invocation hint.

Wait for operator sign-off before flipping `meta.status` to `ready`.

## Output budget

A digital-clone-grade brief lands at **1,500–3,000 lines of YAML**.
Anything < 1,000 suggests under-research (probably stopped at ELT
summary); anything > 4,000 suggests over-collection (probably
inventing personae without `count:` rolling).

## Iterating the skill

When a generated brief looks wrong, **fix this SKILL.md, not the
brief**, then re-invoke. Two runs against the same company should
diff to nothing meaningful except `accessed` dates.

## Re-use targets

Once the brief is `ready`:

1. `compose-org` reads it, forks `zava-control-plane` into
   `zava-control-plane-<slug>`, rebrands `Zava → <Name>` per the
   playbook in `../zava-control-plane/plan/archive/refactor-rebrand-zava-1.md`,
   re-packs `data_fabric/employee_gen.py` SUBSIDIARIES against
   `subsidiaries[]`, replaces hardcoded brand/campaign generators
   with vertical-flavoured ones derived from
   `vertical_entity_kinds[]`, then fans out:
   - `compose-domain` × N for each `proposed_domains[]` entry
   - `compose-persona` × N for each `org_chart[]` entry
   - one `mocks/<id>/` Node server per `stack.systems[]` entry that
     `compose-org` decides is worth a mock
   - cadenced-ritual seeds + narrative-arc seeds for the data fabric.
2. Operator runs the produced `graduate-org.sh` in the new repo.
