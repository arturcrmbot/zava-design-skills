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

## Quickstart — three lines, one prompt, anywhere

You don't need to clone anything. From any empty directory on a
machine with the [GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)
installed:

```bash
mkdir colt-clone && cd colt-clone        # any name; will hold brief + fork
copilot                                   # first run: /login
```

Then in the Copilot session, paste:

```text
Read https://raw.githubusercontent.com/arturcrmbot/zava-design-skills/main/RUNBOOK.md
and follow it end-to-end.

TARGET = colt.net
MODE = full
```

That's it. The agent fetches the runbook + skills + primer from
GitHub raw URLs, walks the `research-company → compose-org`
pipeline, asks you for approval at each destructive step, and
hands you a working fork at `./zava-control-plane-colt/` ready to
`make up`.

**Modes:**

- `MODE = research-only` — just produces the brief; stops there.
- `MODE = full` — produces brief, then forks the substrate.

**Targets:** any company. `colt.net`, `bmw.com`, `ryanair.com`,
`telefonica.com`, `hsbc.com`, etc. The agent picks the matching
[industry primer](skills/research-company/references/industry-primers/)
automatically.

### Install Copilot CLI

If you don't already have it:

```bash
brew install copilot-cli                         # macOS / Linux (Homebrew)
curl -fsSL https://gh.io/copilot-install | bash  # macOS / Linux (script)
npm install -g @github/copilot                   # any platform
winget install GitHub.Copilot                    # Windows
```

You'll need an active [GitHub Copilot subscription](https://github.com/features/copilot/plans).

### What you get

After ~60–90 minutes of wall-clock + your approvals:

```
colt-clone/
├── briefs/
│   └── colt-org-brief.yaml          ← thin company overlay
└── zava-control-plane-colt/         ← rebranded substrate fork
    ├── api/...                      (rebranded, repacked, swapped)
    ├── data/...
    ├── web/...
    └── ... (~2,200 files, ~30-40 atomic commits)
```

`cd zava-control-plane-colt && make up` boots the demo at
`http://localhost:5273`.

## Skills Catalog

### 🔍 Discovery & Profiling

| Skill | Description |
|-------|-------------|
| [**research-company**](skills/research-company/) | Profile a real-world organisation against the public web and emit a thin `org-brief` YAML — the company-specific overlay that pairs with an industry primer to drive a digital-clone-grade substrate fork. |

### 🏗️ Composition

| Skill | Description |
|-------|-------------|
| [**compose-org**](skills/compose-org/) | Fork an agentic substrate into a customer-flavoured digital clone using an `org-brief` + the matching industry primer. Rebrands, repacks the data fabric, swaps entity kinds, regenerates personae/domains, scaffolds stack mocks. Local-only fork by default. |

## How it works under the hood

The Copilot CLI agent fetches the bootstrap doc by URL:

- [`RUNBOOK.md`](RUNBOOK.md) — the master orchestrator the agent
  reads first. Tells it which SKILL.md / primer / schema files to
  fetch (also by URL) and in what order.

Then it walks the two-skill pipeline:

1. **`research-company`** ([SKILL.md](skills/research-company/SKILL.md))
   — read the target's public footprint, emit a thin
   `org-brief.yaml`.
2. **`compose-org`** ([SKILL.md](skills/compose-org/SKILL.md)) —
   read the signed-off brief + matching industry primer, clone the
   substrate, rebrand, repack the data fabric, swap the entity-kind
   schema, generate personae, extend the domain registry, scaffold
   stack mocks.

See [PIPELINE.md](PIPELINE.md) for the cross-skill contract and
detailed runbook.

## Power-user mode — clone the catalog locally

For catalog contributors (editing skills/primers, debugging the
pipeline, working offline) you can also clone the catalog and run
the agent against local files instead of GitHub raw URLs:

```bash
mkdir -p ~/agent-substrate && cd ~/agent-substrate
git clone https://github.com/arturcrmbot/zava-design-skills
git clone https://github.com/arturcrmbot/zava-control-plane
cd zava-design-skills
copilot
```

Then prompt:

```text
Read skills/research-company/SKILL.md and skills/compose-org/SKILL.md
and run them against colt.net (mode=full). Substrate is at
../zava-control-plane.
```

Output lands inside the catalog (`briefs/`, gitignored) and the fork
as a sibling (`../zava-control-plane-colt/`). This is the
contributor's preferred mode because skill edits are picked up
immediately without push-and-re-fetch.

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
