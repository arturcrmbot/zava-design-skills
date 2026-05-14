# 🧬 Zava Design Skills

> Operator-side design-time skills for forking the [zava-control-plane](https://github.com/arturcrmbot/zava-control-plane) substrate into customer-flavoured digital-clone demos.

[![Skills](https://img.shields.io/badge/skills-2-blue)](#skills-catalog)
[![License: MIT](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Convention: awesome-gbb](https://img.shields.io/badge/convention-awesome--gbb-blueviolet)](https://github.com/aiappsgbb/awesome-gbb)

---

## Contents

- [What this catalog is](#what-this-catalog-is)
- [Pipeline](#pipeline)
- [Skills Catalog](#skills-catalog)
- [How to Use](#how-to-use)
- [Repository Structure](#repository-structure)
- [Contributing](#contributing)

---

## What this catalog is

A small **agnostic** skill catalog — modelled on the
[`awesome-gbb`](https://github.com/aiappsgbb/awesome-gbb) skill-catalog
convention — for producing **digital-clone-grade** forks of the
agentic substrate. A "digital clone" means breadth parity with the
substrate's own envelope: 25+ vertical-flavoured workflows, 50–80
personae across a 5–6-tier org chart, 12–18 vertical entity kinds.
Not one or two hero use cases — a credible mini-organisation.

Skills here are **agnostic** (no customer names, no PoC names — see
[AGENTS.md](AGENTS.md) § 2.1). Per-engagement output (briefs for
specific target organisations) lives under `briefs/` and is
**gitignored** by default; that directory is engagement-sensitive.

## Pipeline

```
research-company      → compose-org             → make up
(profile target)        (fork the substrate)      (boot the demo)
```

1. **`research-company`** — read the target's public footprint, emit
   a thin `org-brief.yaml`. Operator reviews + signs off.
2. **`compose-org`** — read the signed-off brief + matching industry
   primer, clone the substrate to a sibling repo, rebrand, repack the
   data fabric, swap the entity-kind schema, generate personae, extend
   the domain registry, scaffold stack mocks.
3. **`make up`** — operator boots the customised fork.

See [PIPELINE.md](PIPELINE.md) for the end-to-end runbook.

## Skills Catalog

### 🔍 Discovery & Profiling

| Skill | Description |
|-------|-------------|
| [**research-company**](skills/research-company/) | Profile a real-world organisation against the public web and emit a thin `org-brief` YAML — the company-specific overlay that pairs with an industry primer to drive a digital-clone-grade substrate fork. |

### 🏗️ Composition

| Skill | Description |
|-------|-------------|
| [**compose-org**](skills/compose-org/) | Fork an agentic substrate into a customer-flavoured digital clone using an `org-brief` + the matching industry primer. Rebrands, repacks the data fabric, swaps entity kinds, regenerates personae/domains, scaffolds stack mocks. Local-only fork by default. |

## How to Use

These skills target the **GitHub Copilot CLI** (`gh copilot`) primarily,
with secondary support for Cursor, VS Code Copilot Chat, and Claude
Code. Skills load from `SKILL.md` by the runtime's standard mechanism.

**Manual invocation (any runtime):**

```bash
# Open a session with this repo in cwd
gh copilot

# Inside the session, point the agent at a skill:
> Read skills/research-company/SKILL.md and run it against colt.net.
```

**Pipe a brief into the next stage (once `compose-org` exists):**

```bash
gh copilot
> Read skills/compose-org/SKILL.md and run it against
> briefs/colt-org-brief.yaml.
```

## Repository Structure

```
zava-design-skills/
├── README.md                  # ← you are here (catalog index)
├── AGENTS.md                  # invariants (read before editing)
├── LICENSE                    # MIT
├── .gitignore                 # ignores briefs/, specs/
├── briefs/                    # per-engagement org briefs (gitignored)
└── skills/
    └── <skill-name>/
        ├── SKILL.md           # frontmatter + procedure
        ├── README.md          # extended docs
        └── references/        # canonical reference data (industry primers, schema)
```

## Contributing

> [!IMPORTANT]
> **Read [AGENTS.md](AGENTS.md) first** before editing any skill. It
> captures the invariants — agnostic wording, frontmatter shape,
> `metadata.version` rules, schema-file canon, the mass-edit safety
> rails — that aren't enforced by CI and have bitten us in production
> (and in our sister catalog, `awesome-gbb`).

1. Fork & branch from `main`.
2. Place new skills under `skills/<your-skill-name>/SKILL.md`.
3. Keep customer/PoC names **out** of skill bodies — they belong in
   the gitignored `briefs/` directory or in a separate per-engagement
   repo.
4. Open a PR.

### Skill quality checklist

- [ ] Strict frontmatter (`name`, `description: >`, `metadata.version`)
- [ ] `description` ≤ 1024 chars including `USE FOR` / `DO NOT USE FOR`
- [ ] No customer names, real GUIDs, real ARM IDs in body
- [ ] Procedure is actionable (the agent *does* the work, not just advises)
- [ ] Tested with at least one runtime

## License

MIT — see [LICENSE](LICENSE).

## See also

- [`awesome-gbb`](https://github.com/aiappsgbb/awesome-gbb) — the
  canonical Microsoft GBB skill catalog. Conventions in this repo
  mirror that one.
- [`zava-control-plane`](https://github.com/arturcrmbot/zava-control-plane)
  — the agentic substrate these skills produce forks of.
