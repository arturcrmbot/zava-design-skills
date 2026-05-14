# research-company

Profile a real-world organisation against the public web and emit a
structured **org-brief** YAML detailed enough to drive a
**digital-clone-grade** fork of an agentic substrate.

See [`SKILL.md`](SKILL.md) for the canonical procedure.

## Files

| File | What it is |
|---|---|
| [`SKILL.md`](SKILL.md) | The thirteen-phase procedure. Strict frontmatter (≤1024 char description, semver). |
| [`references/brief-schema.md`](references/brief-schema.md) | Authoritative schema for the output YAML. |
| [`references/industry-primers/`](references/industry-primers/) | Canonical industry shorthand — regulator IDs, entity kinds, workflow names, KPI sets — per vertical. **Canon** ([AGENTS.md § 2.2](../../AGENTS.md#22--reference-data-files-are-canon--do-not-normalize)) — do not normalize. |

## What's in scope

A "digital clone" means breadth parity with the substrate's own
envelope:

- 25–35 vertical-flavoured workflow domains
- 50–80 personae across a 5–6-tier org chart
- 12–18 vertical entity kinds
- 12–20 third-party stack systems
- 8–20 cadenced rituals
- 4–8 named narrative arcs
- 6–12 HUD-KPI cinematics

Below that envelope the brief isn't a "digital clone" — it's a
hero-story summary, and downstream `compose-org` will produce a
toy-looking fork.

## What's NOT in scope

- Single-process deep dives → use `threadlight-design`
- Pitch decks → use a deck generator
- Code generation → out of scope for any research skill
- Post-hoc workshop summaries → out of scope

## Output

Always writes to `briefs/<slug>-org-brief.yaml` (relative to operator
cwd). `briefs/` is gitignored — per-engagement output stays out of
the public catalog. See [AGENTS.md § 2.5](../../AGENTS.md#25--per-engagement-output-stays-in-briefs).

## Changelog

- **1.0.0** — Initial version. Thirteen-phase procedure, v2 schema,
  five industry primers (telco / airline / auto-oem / banking /
  retail), agnostic body, awesome-gbb-conformant frontmatter.
