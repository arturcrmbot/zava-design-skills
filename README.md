# zava-design-skills

Operator-side design-time skills for the
[`zava-control-plane`](https://github.com/arturcrmbot/zava-control-plane)
substrate. **Not** part of the substrate itself, not committed alongside
it.

## What lives here

Skills the operator runs from a Copilot CLI / IDE session to **produce a
new customer-flavoured fork** of the Zava control plane (e.g. a Colt /
Ryanair / BMW / Telefónica clone). The substrate proper ships with its
own design-time skills under `docs/superpowers/skills/` — those extend a
single repo. The skills here are different: they *generate forks*.

| Skill | Role |
|---|---|
| [`skills/research-company/`](skills/research-company/) | Profile a real-world organisation against the public web; emit a structured `org-brief.yaml` for compose-org to consume. |
| `skills/compose-org/` *(to be authored)* | Fork the zava-control-plane repo into a customer-flavoured clone using an org-brief. |

## Cut: why a sibling repo?

- The substrate ships clean — these tools are operator infrastructure,
  not product surface.
- A Colt brief / Ryanair brief / BMW brief is **engagement-sensitive**.
  Keeping them out of the substrate's public history is the safe default.
- Tools here evolve independently of the substrate's release cadence.

## Use

```bash
# In a Copilot CLI session, with cwd inside the *substrate* repo:
> Run `research-company` (from ../zava-design-skills/skills/research-company/SKILL.md)
> on `colt.net`. Write the output to ../zava-design-skills/specs/colt-org-brief.yaml.
```

Outputs land under [`specs/`](specs/) in this repo.

## Layout

```
zava-design-skills/
├─ skills/                — design-time skills (one folder each)
│  └─ research-company/   — SKILL.md + brief.schema.yaml + README.md
├─ specs/                 — output briefs, one per target company
└─ README.md
```

## Local-only by default

This repo is local-only. No GitHub remote is configured by default.
If you want to share a brief with a colleague, push to a private
fork or zip it.

## Pairs with

- [`zava-control-plane`](../zava-control-plane) — the substrate
  these skills generate forks of.
