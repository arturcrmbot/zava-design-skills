# research-company

Design-time skill: profile a real organisation against the public web
and emit a structured `org-brief` YAML detailed enough to drive a
**digital-clone-grade** fork of the `zava-control-plane` substrate.

## When to use

When a colleague says *"I want to demo the substrate flavoured for
&lt;Customer&gt;"*. The brief produced here is the single input to
`compose-org` (the sibling skill, to be authored), which forks the
substrate repo into `zava-control-plane-<slug>`.

## How to invoke

In a Copilot CLI session, with cwd anywhere on the filesystem:

> Read [`SKILL.md`](SKILL.md) and run it against `colt.net`. Write
> output to `specs/colt-org-brief.yaml` in this repo.

The agent runs the thirteen-phase procedure end-to-end.

## What the output looks like

A YAML file of roughly **1,500–3,000 lines** containing:

- identity / ownership / size / geo
- 8–15 subsidiaries
- 10–12 functions
- **40–80** personae across a 5–6-tier org chart
- 8–18 vertical entity kinds
- **20–40** vertical-flavoured workflow domains
- 12–20 third-party stack systems
- 10–20 regulators
- 8–20 cadenced rituals
- 4–8 named narrative arcs
- 6–12 HUD-KPI cinematics
- 10+ cited sources, with `confidence:` on every fact

Below that breadth, the brief isn't a "digital clone" — it's a
hero-story summary.

## Files

| File | What it is |
|---|---|
| [`SKILL.md`](SKILL.md) | The thirteen-phase procedure the agent follows. |
| [`brief.schema.yaml`](brief.schema.yaml) | Authoritative schema (v2) for the output. |

## Output convention

Always writes to `../../specs/<slug>-org-brief.yaml` (relative to
this skill folder; the `specs/` directory in the repo root).

Never writes anywhere else.
