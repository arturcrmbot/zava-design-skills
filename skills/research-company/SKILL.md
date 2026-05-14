---
name: research-company
description: >
  Profile an organisation from public web sources and emit an
  org-brief YAML detailed enough to drive a digital-clone-grade
  substrate fork — 25–35 vertical-flavoured workflow domains, 50–80
  personae across a 5–6-tier org chart, 12–18 vertical entity kinds,
  12–20 stack systems, 8–20 cadenced rituals, 4–8 narrative arcs.
  Thirteen-phase procedure; claims carry confidence (high/medium/low/
  inferred/unknown) + sources[]; gaps go in uncertainties[], never
  invented. Any vertical (Telco, FSI, MFG, Retail, Airline, Auto,
  Healthcare, Utilities).
  USE FOR: profile a company before a customer pilot, generate an
  org-brief, research a target organisation, prepare a customer-
  flavoured demo fork, industry-realism research.
  DO NOT USE FOR: single-process deep design (use threadlight-design),
  pitch-deck authoring, code generation.
metadata:
  version: "1.0.0"
---

# research-company

Profile a real-world organisation against the public web and produce a
structured **org-brief** YAML detailed enough to drive a
**digital-clone-grade** fork of an agentic substrate.

A "digital clone" means breadth parity with the substrate's own
envelope — 25+ workflow domains, 50–80 personae across a 5–6-tier org
chart, 12–18 vertical entity kinds. Not one or two hero use cases; a
credible mini-organisation that someone from the target industry would
recognise.

## When to Use

Invoke this skill when the user wants to:

- Profile a target organisation before a customer pilot or workshop
- Spin up a customer-flavoured demo fork of an existing substrate
- Produce a reviewable spec a customer SME can sanity-check for
  industry realism
- Prepare onboarding context for a colleague handed a new account

## Two modes

| Mode | When | Flow |
|---|---|---|
| **Full** | Customer SME will review for realism; multi-day engagement | Run all 13 phases; produce 1,500–3,000 line brief |
| **Fast-PoC** | Demo dry run; quick scope check; weekend hack | Phases A–F + I + L only; produce 600–1,200 line brief |

Default to Full. The user can request Fast-PoC explicitly ("quick
profile", "fast PoC scoping").

## Output convention

```
briefs/<slug>-org-brief.yaml
```

where `<slug>` is the kebab-case form of the target organisation's
short name (≤ 16 chars). The path is **relative to the operator's
working directory** by default — typically the catalog repo root.

`briefs/` is gitignored in this catalog (see [AGENTS.md § 2.5](../../AGENTS.md#25--per-engagement-output-stays-in-briefs)).
Per-engagement briefs do not ship with the public skill catalog.

## Inputs

Invoke one of two ways:

1. **Company name or web domain.** "Run `research-company` on
   `<target>.com`." Pick a `slug` (kebab-case, ≤ 16 chars). Read the
   slug back to the operator if ambiguous (e.g. `apple` vs
   `apple-records`).
2. **Existing in-progress brief.** Resume at the lowest unfilled
   phase.

## Tooling priority

Every factual claim must reference one or more rows in `sources[]`,
gathered via these tools in this priority order:

1. **`web_fetch` against the target's own properties** — `/about`,
   `/leadership`, `/investors`, latest annual report or capital-
   markets-day PDF, `/press` or `/newsroom`, `/sustainability`,
   `/governance`. Self-reported data carries `confidence: medium`
   unless cross-corroborated.
2. **`web_search` for cross-corroboration.** Two independent
   secondary sources concurring lifts `confidence` to `high`.
3. **`web_fetch` against authoritative third parties.** Wikipedia
   (factual scaffolding), national registry (Companies House / SEC
   EDGAR / Handelsregister / Infogreffe / equivalent), regulator
   filings, theofficialboard.com, theorg.com, equilar.com, news of
   record.
4. **`web_search` for stack signal.** Company jobs board ("we use
   Salesforce, ServiceNow…"), vendor case-study pages, Gartner /
   Forrester / Omdia write-ups.
5. **`web_search` for organisational depth.** LinkedIn company search
   ("VP at <target>", "Director of Y at <target>"), trade-press
   leadership profiles, business-school case studies. **This is the
   phase that gets the brief from ELT-summary to 60+ personae.**

Never rely on a single answer-engine summary; every claim must point
at one or more rows in `sources[]`.

## Output schema

The output conforms to the schema in
[`references/brief-schema.md`](references/brief-schema.md). Read it
once before you begin. Key rules:

- Every `Fact` requires `{value, confidence, source_refs?, notes?}`.
- Every `Source` row needs `{id, url, accessed, kind, used_for}`.
- Every truly-missing field is recorded in `uncertainties[]`.
- `meta.status` walks `in_progress` → `needs_review` → `ready`.

## Industry primers

Before you start a thirteen-phase walk for a target in a known
vertical, **skim the matching industry primer** in
[`references/industry-primers/`](references/industry-primers/). The
primer captures canonical regulator IDs, standard entity-kind sets,
typical workflow names, and the published shorthand used in that
industry's spec language.

| Vertical | Primer |
|---|---|
| Telco | [`references/industry-primers/telco.md`](references/industry-primers/telco.md) |
| Airline | [`references/industry-primers/airline.md`](references/industry-primers/airline.md) |
| Automotive (OEM) | [`references/industry-primers/auto-oem.md`](references/industry-primers/auto-oem.md) |
| FSI (banking) | [`references/industry-primers/banking.md`](references/industry-primers/banking.md) |
| Retail | [`references/industry-primers/retail.md`](references/industry-primers/retail.md) |

If no primer exists, work from first principles — the procedure
below is vertical-agnostic; primers just accelerate the
operationally-loaded phases (G, H, J, K).

> Primers are **canon** — see [AGENTS.md § 2.2](../../AGENTS.md#22--reference-data-files-are-canon--do-not-normalize).
> Do not normalize the published shorthand they document.

---

## The thirteen phases

### Phase 0 — Bootstrap

Create the spec stub at `briefs/<slug>-org-brief.yaml` with skeleton
keys + empty arrays. All later phases append via `edit`.

### Phase A — Identity & description

Wikipedia + the company's own `/about`. Fill
`identity.{name, short_name, slug, domain, description, brand_voice,
industry, sub_industry, tagline}`.

`brand_voice`: read three recent press releases or the CEO's last
capital-markets statement and characterise the company's tone in two
lines. Cite both sources.

### Phase B — Ownership & size

SEC EDGAR / Companies House / annual report. Fill
`ownership.{structure, parent, ticker, founded, key_shareholders}`
and `size.{employees, revenue_usd, revenue_currency, revenue_period,
customers_count, assets_count}`.

`size.assets_count` is the **vertical-native scale signal** — on-net
buildings for telco; aircraft in fleet for airline; dealers for auto
OEM; branches for retail bank; plants for FMCG. Don't skip — this
is what the demo dashboards anchor on.

### Phase C — Geography

`geo.{hq, regions, countries_present, key_hubs, footprint_notes}`.
`footprint_notes[]` collects vertical-native callouts ("250+ cloud
PoPs", "1,300 dealers", "180 routes flown") — material for the demo
HUD.

### Phase D — Subsidiaries & legal entities

Walk the national registry. Cap at 15 — the holding plus the
materially-sized opcos. Each gets `{id, name, country, role,
regulator, confidence}`.

### Phase E — Functions

Distil 10–12 canonical functions. AT MOST 3 `importance: hero`. The
function tree should cover at minimum: finance, hr,
legal-and-regulatory, sales, the operational hero function for the
vertical (per industry primer), customer-success, tech, security.

### Phase F — Org chart depth (the big phase)

**This is the phase that takes the brief from "ELT summary" to
"digital clone."** Produce 50–80 `PersonaNode` rows in `org_chart[]`
covering EVERY function. For each function:

- the function head (level 2 — already on `/leadership`)
- the SVPs / Directors that report to them (level 3)
- one or two regional or sub-team leads (level 4–5)
- 1–3 individual-contributor archetypes with `count` set (level 6)

Sources:

- `/leadership` for level 1–2.
- LinkedIn search for level 3–5 ("Director of <X> at <target>",
  "Head of <Y> at <target>").
- Annual report management-team list.
- theofficialboard.com / theorg.com for free-text reports.
- Trade-press leadership interviews — they often reveal sub-team
  structure.

Use `count: N` for "N of these exist" so 60-row breadth doesn't
require inventing 60 names. Real names go on level 1–2 only; below
that, `name` stays empty and the persona is "the role" not "the
person."

Validation: every function in `functions[]` has ≥3 personae rows;
hero functions have ≥6.

### Phase G — Vertical entity kinds

12–18 rows in `vertical_entity_kinds[]`. Cover the agency-table
swaps (`Brand`, `Campaign`, `Pitch`, `MediaPlan`) plus any net-new
kinds the vertical needs. See your industry primer for the standard
set; adapt to what the target actually publishes.

### Phase H — Proposed domains (the breadth phase)

**Produce 25–35 rows in `proposed_domains[]`** spanning every
function. ≤ 3 hero. Avoid duplicating generic substrate workflows
(expense-claim, hiring, vendor-kyc, ap-invoice, …) that apply
universally — only list vertical-specific workflows.

Method:

1. List the functions.
2. For each function, brainstorm 2–5 workflows specific to the
   vertical at that function. (See industry primer for a starting
   list.)
3. Add 3–5 cross-function meta-workflows (e.g.
   `<integration-name>-cutover` touches tech + customer-success +
   ops).
4. Mark each with `cadence:` (ad-hoc / daily / weekly / monthly /
   quarterly / annual). Drives the simulator's per-workflow spawn
   rate.

Validation: every function has ≥2 domain rows; hero functions ≥4.

### Phase I — Stack

12–20 rows in `stack.systems[]`. Three angles:

1. **Vendor case-study pages** (Salesforce / ServiceNow / SAP /
   Microsoft / vertical-native vendors all publish logo walls) →
   `confidence: high` when the target's logo appears.
2. **Company jobs board** — `site:careers.<domain> Salesforce` etc.
   `confidence: medium`.
3. **Industry-standard inference** — list the role and 2–3
   `candidates[]` with `confidence: inferred`.

Always include: CRM, ITSM, ERP, HCM, identity (often Entra ID at
Microsoft-heavy customers), esign, observability, and the
vertical-native operational system from your industry primer
(OSS/BSS, reservations, MES + PLM, OMS, core-banking, etc.).

### Phase J — Regulators

One row per country in `geo.countries_present` × material regulatory
domain. Add horizontal regulators (data-protection, anti-trust) for
the HQ jurisdiction. See industry primer for the standard regulator
set per country.

### Phase K — Cadenced rituals

12–20 rows in `cadenced_rituals[]`. Cover at least one ritual per
function. Examples: daily NOC shift handover (telco); weekly capacity
review; monthly RFO close; quarterly regulator filing; annual
licence renewal; annual budget cycle; quarterly board pack; weekly
sales pipeline scrub. Each row gets `cadence:`.

### Phase L — Narrative arcs & KPIs

4–8 named `narrative_arcs[]` drawn from the last 24 months of press
+ analyst coverage. Each gets a `headline`, a `summary`, the
`function_focus[]` it touches, a `timeline_hint`.

Then 6–12 `kpi_cinematics[]` rows — the HUD KPIs the vertical
demands. See industry primer for the standard KPI set.

### Phase M — Cross-check & finalise

Pick **five random `confidence: high` claims**. Re-run `web_search`
and confirm. Downgrade any that don't recheck.

Validate against the schema:

- All `minItems` constraints satisfied (`org_chart[] ≥ 40`,
  `proposed_domains[] ≥ 20`, `vertical_entity_kinds[] ≥ 8`,
  `stack.systems[] ≥ 10`, `cadenced_rituals[] ≥ 8`,
  `narrative_arcs[] ≥ 3`, `kpi_cinematics[] ≥ 6`).
- Every Fact has `confidence`.
- Every `source_refs[]` entry exists in `sources[]`.
- `meta.status` → `needs_review`.

Print a short summary:

- counts: subsidiaries N, personae N, proposed_domains N,
  cadenced_rituals N, narrative_arcs N, sources N
- 5 highest-confidence claims
- 5 most material uncertainties
- a one-line hint for the downstream `compose-org` skill

Wait for operator sign-off before flipping `meta.status` to `ready`.

## Output budget

A digital-clone-grade brief lands at **1,500–3,000 lines of YAML**.
< 1,000 → under-research (probably stopped at ELT summary).
> 4,000 → over-collection (probably inventing personae without
`count:` rolling).

## Iterating the skill

When a generated brief looks wrong, **fix this SKILL.md, not the
brief**, then re-invoke. Two runs against the same target should
diff to nothing meaningful except `accessed` dates.

## Downstream re-use

Once `meta.status: ready`, hand off to `compose-org` (sibling skill,
to be authored). That skill consumes the brief and forks a target
substrate repo into a customer-flavoured clone.
