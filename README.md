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

## Quickstart — Try it end-to-end

Clone this catalog and a substrate repo side by side, then drive the
agent with the prompt below. The pipeline is **agent-driven but
operator-gated** — you approve every destructive step before it
runs.

### Prerequisites

```bash
# Install GitHub Copilot CLI (one of these):
brew install copilot-cli                         # macOS / Linux (Homebrew)
curl -fsSL https://gh.io/copilot-install | bash  # macOS / Linux (script)
npm install -g @github/copilot                   # any platform
winget install GitHub.Copilot                    # Windows

# Layout: both repos as siblings under one parent directory.
mkdir -p ~/agent-substrate && cd ~/agent-substrate
git clone https://github.com/arturcrmbot/zava-design-skills
git clone https://github.com/arturcrmbot/zava-control-plane

# Other tools the substrate fork will need at boot
git --version
python3 --version       # ≥ 3.11
node --version          # ≥ 20
```

### Step 1 — Open a Copilot CLI session inside the catalog

```bash
cd ~/agent-substrate/zava-design-skills
copilot                  # first run will prompt /login
```

### Step 2 — Paste the runbook prompt

Replace `<TARGET>` with the company you want to clone (a domain name
works best, e.g. `colt.net`, `ryanair.com`, `bmw.com`,
`telefonica.com`). Then paste:

```text
You are exercising the two-skill design-time pipeline in this repo
(research-company → compose-org) that forks an agentic substrate into a
customer-flavoured digital clone.

TARGET = <TARGET>

READ FIRST, IN THIS ORDER (paths are relative to this repo root):
  1. PIPELINE.md
  2. AGENTS.md
  3. skills/research-company/SKILL.md
  4. skills/compose-org/SKILL.md
  5. skills/research-company/references/industry-primers/README.md
     (then the matching primer once you know the target's vertical)

Substrate path: ../zava-control-plane (sibling of this repo). Assume
clean main branch.

YOUR TASK

Step 1 — Run research-company against TARGET.
  - Walk all four phases per skills/research-company/SKILL.md.
  - Write briefs/<slug>-org-brief.yaml (gitignored).
  - meta.status walks in_progress → needs_review.

Step 2 — STOP. Print to me:
  - 3 highest-confidence claims
  - 5 most material uncertainties
  - 3 random `confidence: high` cross-check spot-results
  Wait for my "ok, proceed" before any further write.

Step 3 — After my go-ahead, flip meta.status: ready and set
  meta.reviewer to a one-line note ("smoke test YYYY-MM-DD").

Step 4 — Run compose-org against the brief.
  - Fork target: ../zava-control-plane-<slug>
  - Walk Phase 0 → Phase J sequentially.
  - After every phase print: one-line summary, last commit SHA,
    files-touched count.

Step 5 — STOP AND ASK ME before each destructive step:
  - Phase A (git clone)
  - Phase B (rebrand find-and-replace — show me the find/replace
    mapping table first)
  - Phase D (Kuzu schema swap)
  - Phase H (data-fabric re-seed)
  - Phase I (`make test`)

Step 6 — If any phase fails, STOP. Do not improvise. Report:
  - which phase + sub-step
  - what command/diff failed
  - which line of SKILL.md, primer, or brief is unclear or wrong
  I will fix the skill/primer/brief; you re-run.

CONSTRAINTS
  - NO `git push`, NO `gh repo create`. Local-only.
  - NEVER edit the substrate at ../zava-control-plane itself. Only
    the new fork dir.
  - NEVER edit a skill, primer, or brief silently mid-run. If you
    find them wrong, stop and tell me.
  - Stay agnostic in commit messages.
  - If the SKILL.md says X, do X verbatim. If X is unclear, ask.

START NOW with the read-and-summarise step. Wait for "go" before
any write.
```

### What success looks like

After ~60–90 minutes of wall-clock + your approvals at each gate:

- `../zava-design-skills/briefs/<slug>-org-brief.yaml` — committed
  facts about TARGET, `meta.status: ready`.
- `../zava-control-plane-<slug>/` — a local git repo with ~30–40
  atomic commits; substrate rebranded; data fabric repacked; Kuzu
  schema swapped; personae generated; ~25+ stub domains added;
  Node mocks scaffolded.
- `make up` in the fork boots a customised demo at
  `http://localhost:5273`.

### When something breaks

The pipeline is v1.0.0. Expect rough edges. When the agent stops
with "this phase failed because line N of SKILL.md is unclear":

- Edit the skill or primer to fix the underlying issue.
- Re-run with `--allow-overwrite`.
- Commit your skill/primer fix back to this catalog.

Repeating the bug fix in another fork is wasted work; capture it
once.

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

These skills target the **GitHub Copilot CLI** (`copilot`) primarily,
with secondary support for Cursor, VS Code Copilot Chat, and Claude
Code. Skills load from `SKILL.md` by the runtime's standard mechanism.

**Manual invocation** is documented in the
[Quickstart](#quickstart--try-it-end-to-end) above. The two-skill chain
is also documented in detail in [PIPELINE.md](PIPELINE.md).

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
