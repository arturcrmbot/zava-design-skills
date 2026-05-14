# Brief Schema

Authoritative output schema for the
[`research-company`](../SKILL.md) skill. Every brief produced by the
skill conforms to the YAML below. Schema version is `2`.

## Design intent

A brief that conforms to this schema is detailed enough to drive a
**digital-clone-grade** fork of an agentic substrate. Breadth
parity envelope:

- 25–35 vertical-flavoured workflow domains
- 50–80 personae across a 5–6-tier org chart
- 10–12 canonical functions
- 12–18 vertical entity kinds
- 15–25 cadenced rituals + named narrative arcs
- 12–20 third-party stack systems

## Output path

`briefs/<slug>-org-brief.yaml` (relative to operator cwd). The
`briefs/` directory is gitignored in the catalog repo — per-engagement
output never ships with the public skill catalog.

## Confidence discipline

Every factual field MUST carry a `confidence` discriminator:

| Level | Meaning |
|---|---|
| `high` | Two or more independent public sources agree |
| `medium` | One authoritative source (company site, annual report, regulator filing) |
| `low` | One secondary source (analyst aggregator, news article) |
| `inferred` | Deduced from vertical / industry pattern, not directly stated |
| `unknown` | No public information found; logged in `uncertainties[]` |

Gaps land in `uncertainties[]` — never invented.

## Schema (JSON-Schema 2020-12, expressed as YAML)

```yaml
$schema: "https://json-schema.org/draft/2020-12/schema"
$id: "https://zava-design-skills.dev/research-company/brief.schema.yaml"
title: "research-company org-brief"
type: object
required: [meta, identity, ownership, size, geo, sources]
additionalProperties: false

# ---------------------------------------------------------------- $defs
$defs:

  Confidence:
    type: string
    enum: [high, medium, low, inferred, unknown]

  Fact:
    type: object
    required: [value, confidence]
    additionalProperties: false
    properties:
      value:        {}                                   # scalar or list
      confidence:   { $ref: "#/$defs/Confidence" }
      source_refs:  { type: array, items: { type: string } }    # ids from `sources[]`
      notes:        { type: string }

  Source:
    type: object
    required: [id, url, accessed]
    additionalProperties: false
    properties:
      id:           { type: string, pattern: "^[a-z][a-z0-9-]*$" }
      url:          { type: string, format: uri }
      accessed:     { type: string, format: date }
      kind:
        type: string
        enum: [official-site, wikipedia, annual-report, regulator, analyst, news, jobs-board, linkedin, org-chart, blog, press-release, vendor-case-study, other]
      used_for:     { type: array, items: { type: string } }

  Uncertainty:
    type: object
    required: [field, note]
    additionalProperties: false
    properties:
      field:        { type: string }
      note:         { type: string }
      followup:     { type: string }

  PersonaNode:
    type: object
    required: [id, title, function, level, confidence]
    additionalProperties: false
    properties:
      id:           { type: string, pattern: "^[a-z][a-z0-9_]*$" }
      title:        { type: string }
      name:         { type: string }                # only for public level-1/2 leaders
      function:     { type: string }                # id from `functions[]`
      level:
        type: integer
        minimum: 1
        maximum: 6
      reports_to:   { type: string }                # another persona's id
      count:
        type: integer
        minimum: 1
        description: "How many of this role exist (e.g. 8 NOC controllers)."
      confidence:   { $ref: "#/$defs/Confidence" }
      source_refs:  { type: array, items: { type: string } }
      notes:        { type: string }

# ---------------------------------------------------------------- properties
properties:

  meta:
    type: object
    required: [generated_by, generated_at, status, schema_version]
    additionalProperties: false
    properties:
      generated_by:    { const: "research-company" }
      generated_at:    { type: string, format: date-time }
      status:
        type: string
        enum: [in_progress, ready, needs_review]
      schema_version:  { const: 2 }
      reviewer:        { type: string }

  identity:
    type: object
    required: [name, slug, description]
    additionalProperties: false
    properties:
      name:            { type: string }
      slug:            { type: string, pattern: "^[a-z][a-z0-9-]*$" }
      short_name:      { type: string }
      domain:          { type: string }
      description:     { $ref: "#/$defs/Fact" }
      brand_voice:     { $ref: "#/$defs/Fact" }
      industry:        { $ref: "#/$defs/Fact" }
      sub_industry:    { $ref: "#/$defs/Fact" }
      tagline:         { $ref: "#/$defs/Fact" }
      ticker_symbols:  { $ref: "#/$defs/Fact" }

  ownership:
    type: object
    required: [structure]
    additionalProperties: false
    properties:
      structure:       { $ref: "#/$defs/Fact" }
      parent:          { $ref: "#/$defs/Fact" }
      ticker:          { $ref: "#/$defs/Fact" }
      founded:         { $ref: "#/$defs/Fact" }
      key_shareholders:
        type: array
        items: { $ref: "#/$defs/Fact" }

  size:
    type: object
    additionalProperties: false
    properties:
      employees:       { $ref: "#/$defs/Fact" }
      revenue_usd:     { $ref: "#/$defs/Fact" }
      revenue_currency: { $ref: "#/$defs/Fact" }
      revenue_period:  { $ref: "#/$defs/Fact" }
      customers_count: { $ref: "#/$defs/Fact" }
      assets_count:
        $ref: "#/$defs/Fact"
        description: |
          Vertical-native scale signal: on-net buildings (telco),
          aircraft (airline), dealers (auto OEM), branches (retail
          bank), plants (FMCG).

  geo:
    type: object
    required: [hq]
    additionalProperties: false
    properties:
      hq:              { $ref: "#/$defs/Fact" }
      regions:
        type: array
        items: { $ref: "#/$defs/Fact" }
      countries_present: { $ref: "#/$defs/Fact" }
      key_hubs:
        type: array
        items: { $ref: "#/$defs/Fact" }
      footprint_notes:
        type: array
        items: { $ref: "#/$defs/Fact" }

  subsidiaries:
    type: array
    maxItems: 15
    items:
      type: object
      required: [id, name, confidence]
      additionalProperties: false
      properties:
        id:            { type: string, pattern: "^[a-z][a-z0-9-]*$" }
        name:          { type: string }
        country:       { type: string }
        role:
          type: string
          enum: [holding, opco, holding+opco, regional-hub, joint-venture, dormant, financing-vehicle]
        regulator:     { type: string }
        confidence:    { $ref: "#/$defs/Confidence" }
        source_refs:   { type: array, items: { type: string } }
        notes:         { type: string }

  functions:
    description: "10–12 canonical functions; ≤ 3 hero."
    type: array
    minItems: 8
    maxItems: 14
    items:
      type: object
      required: [id, importance]
      additionalProperties: false
      properties:
        id:            { type: string, pattern: "^[a-z][a-z0-9-]*$" }
        display_name:  { type: string }
        importance:
          type: string
          enum: [hero, core, support]
        leader_persona_id: { type: string }
        notes:         { type: string }

  org_chart:
    description: |
      Flat list of 50–80 PersonaNode rows; linked by `reports_to` to
      form a 5–6-tier tree. Each function in `functions[]` has ≥3
      personae; hero functions ≥6.
    type: array
    minItems: 40
    maxItems: 100
    items: { $ref: "#/$defs/PersonaNode" }

  vertical_entity_kinds:
    description: "Kuzu-style node-kind changes; cap 18. See industry primers."
    type: array
    minItems: 8
    maxItems: 18
    items:
      type: object
      required: [kind, description, swaps_for]
      additionalProperties: false
      properties:
        kind:          { type: string }
        swaps_for:
          type: string
          enum: [Brand, Campaign, Pitch, MediaPlan, Subsidiary, "(new)"]
        description:   { type: string }
        relations:
          type: array
          items: { type: string }

  proposed_domains:
    description: |
      Vertical-flavoured workflow types. **25–35 rows** spanning every
      function. ≤ 3 hero. Generic substrate workflows (expense-claim,
      hiring, vendor-kyc, ap-invoice…) are NOT listed here — only
      vertical-specific ones.
    type: array
    minItems: 20
    maxItems: 40
    items:
      type: object
      required: [workflow_type, display_name, importance, summary, function]
      additionalProperties: false
      properties:
        workflow_type: { type: string, pattern: "^[a-z][a-z0-9-]*$" }
        display_name:  { type: string }
        importance:
          type: string
          enum: [hero, supporting]
        summary:       { type: string }
        function:      { type: string }
        cadence:
          type: string
          enum: [ad-hoc, daily, weekly, monthly, quarterly, annual]

  stack:
    type: object
    additionalProperties: false
    properties:
      systems:
        type: array
        minItems: 10
        maxItems: 22
        items:
          type: object
          required: [id, role, confidence]
          additionalProperties: false
          properties:
            id:        { type: string, pattern: "^[a-z][a-z0-9-]*$" }
            role:
              type: string
              enum:
                - crm
                - itsm
                - erp
                - hcm
                - network-vendor
                - esign
                - identity
                - dwh
                - lakehouse
                - observability
                - ticketing
                - billing
                - oss-bss
                - scm
                - ecommerce
                - marketing
                - design
                - reservations
                - mes
                - plm
                - core-banking
                - payments
                - oms
                - cdn
                - dns
                - field-service
                - other
            vendor:    { type: string }
            confidence: { $ref: "#/$defs/Confidence" }
            source_refs: { type: array, items: { type: string } }
            candidates:
              type: array
              items: { type: string }
            notes:     { type: string }

  regulators:
    type: array
    items:
      type: object
      required: [id, name, country, domain, confidence]
      additionalProperties: false
      properties:
        id:            { type: string, pattern: "^[a-z][a-z0-9-]*$" }
        name:          { type: string }
        country:       { type: string }
        domain:
          type: string
          enum: [telecom, aviation, finance, banking, securities, insurance, energy, environmental, data-protection, anti-trust, food-safety, pharma, automotive, broadcasting, gaming, customs, labour, taxation, healthcare, defence]
        confidence:    { $ref: "#/$defs/Confidence" }

  customers_or_segments:
    type: object
    additionalProperties: false
    properties:
      mode:
        type: string
        enum: [b2c, b2b, b2b2c, mixed]
      customer_segments:
        type: array
        items: { $ref: "#/$defs/Fact" }
      key_accounts:
        type: array
        items: { $ref: "#/$defs/Fact" }

  market_position:
    type: object
    additionalProperties: false
    properties:
      competitors:
        type: array
        items: { $ref: "#/$defs/Fact" }
      segment_blurb: { $ref: "#/$defs/Fact" }
      moats:
        type: array
        items: { $ref: "#/$defs/Fact" }

  strategic_themes:
    description: "5–8 themes from the last 24 months of press / annual reports."
    type: array
    minItems: 4
    maxItems: 10
    items: { $ref: "#/$defs/Fact" }

  cadenced_rituals:
    description: |
      Periodic rituals the org runs. Aim for 12–20 rows spanning
      daily / weekly / monthly / quarterly / annual.
    type: array
    minItems: 8
    maxItems: 25
    items:
      type: object
      required: [id, display_name, cadence, summary]
      additionalProperties: false
      properties:
        id:            { type: string, pattern: "^[a-z][a-z0-9-]*$" }
        display_name:  { type: string }
        cadence:
          type: string
          enum: [daily, weekly, biweekly, monthly, quarterly, semiannual, annual]
        summary:       { type: string }
        owner_function: { type: string }
        confidence:    { $ref: "#/$defs/Confidence" }

  narrative_arcs:
    description: "Named storylines drawn from real recent events. 4–8 rows."
    type: array
    minItems: 3
    maxItems: 10
    items:
      type: object
      required: [id, headline, summary, function_focus, confidence]
      additionalProperties: false
      properties:
        id:            { type: string, pattern: "^[a-z][a-z0-9-]*$" }
        headline:      { type: string }
        summary:       { type: string }
        function_focus:
          type: array
          items: { type: string }
        timeline_hint: { type: string }
        confidence:    { $ref: "#/$defs/Confidence" }
        source_refs:   { type: array, items: { type: string } }

  kpi_cinematics:
    description: "HUD KPIs the vertical demands. 6–12 rows."
    type: array
    minItems: 6
    maxItems: 15
    items:
      type: object
      required: [id, display_name, unit, target_trend]
      additionalProperties: false
      properties:
        id:            { type: string, pattern: "^[a-z][a-z0-9_]*$" }
        display_name:  { type: string }
        unit:
          type: string
          enum: [percent, count, ratio, currency_usd, currency_local, ms, hours, days, mbps, gbps, tbps, "n/a"]
        target_trend:
          type: string
          enum: [up_is_good, down_is_good, stable]
        owner_function: { type: string }
        notes:         { type: string }

  sources:
    type: array
    minItems: 10
    items: { $ref: "#/$defs/Source" }

  uncertainties:
    type: array
    items: { $ref: "#/$defs/Uncertainty" }
```

## Cross-references in the brief

- `subsidiaries[].regulator` → an `id` in `regulators[]`
- `functions[].leader_persona_id` → an `id` in `org_chart[]`
- `org_chart[].function` → an `id` in `functions[]`
- `org_chart[].reports_to` → another `org_chart[].id`
- `proposed_domains[].function` → an `id` in `functions[]`
- `cadenced_rituals[].owner_function` → an `id` in `functions[]`
- `narrative_arcs[].function_focus[]` → `id`s in `functions[]`
- `*.source_refs[]` → `id`s in `sources[]`

The brief is considered valid only when every cross-reference
resolves.
