# RUNBOOK — Bootstrap an organisational digital clone from a fresh directory

> The single entry point for the `zava-design-skills` catalog. An
> agent invoked from any empty directory can fetch this file by URL,
> follow it end-to-end, and produce a complete customer-flavoured
> fork of the substrate without anything pre-installed beyond the
> Copilot CLI itself.

---

## What this runbook does

You (the agent) read this file plus a small set of SKILL.md /
primer / schema files **directly from GitHub raw URLs** — nothing
pre-cloned on disk. You then walk the two-skill pipeline
(`research-company` → `compose-org`) end-to-end. The output lands
in the operator's current working directory:

```
<cwd>/
├── briefs/
│   └── <slug>-org-brief.yaml         ← from research-company
└── zava-control-plane-<slug>/        ← from compose-org (a fresh fork)
```

## Inputs the operator gives you

The operator will provide, in their prompt:

- **TARGET** — a company name or web domain (e.g. `colt.net`,
  `bmw.com`, `ryanair.com`). Required.
- **MODE** — one of:
  - `research-only` — produce only the brief; stop after step 4.
  - `full` (default) — produce brief, then fork the substrate.

If they didn't specify MODE, default to `full` and ask them to
confirm before any destructive step (see § Safety gates).

## Catalog file URLs

Fetch these via `web_fetch` against the raw URL pattern:

```
https://raw.githubusercontent.com/arturcrmbot/zava-design-skills/main/<path>
```

You will need:

| Purpose | Path |
|---|---|
| Contributor invariants | `AGENTS.md` |
| Pipeline overview | `PIPELINE.md` |
| **Skill 1: profile the target** | `skills/research-company/SKILL.md` |
| Brief schema (read-once reference) | `skills/research-company/references/brief-schema.md` |
| Industry primer (pick one based on TARGET's vertical) | `skills/research-company/references/industry-primers/<vertical>.md` |
| **Skill 2: fork the substrate** | `skills/compose-org/SKILL.md` |

Industry-primer choices (pick by inspection of the target's site):
`telco`, `airline`, `auto-oem`, `banking`, `retail`. If unsure, fetch
`skills/research-company/references/industry-primers/README.md` first.

Substrate URL (the repo that gets forked):
`https://github.com/arturcrmbot/zava-control-plane`.

## Procedure

### Step 1 — Boot context

Fetch and read, in this order:

1. `AGENTS.md` — invariants you must obey (no customer names in
   skills/refs you write, etc.).
2. `PIPELINE.md` — pipeline overview and cross-skill contract.
3. `skills/research-company/SKILL.md` — the procedure for skill 1.
4. `skills/research-company/references/brief-schema.md` — the output
   schema. Treat this as the contract.
5. The matching industry-primer file. Telco for connectivity /
   carrier / colocation; airline for any IATA carrier; banking for
   FSI; auto-oem for vehicle manufacturers; retail for grocers,
   apparel, e-commerce.

If MODE is `full`, also fetch:

6. `skills/compose-org/SKILL.md` — the procedure for skill 2.

### Step 2 — Run research-company

Walk all four phases per the SKILL.md you just fetched. Output
goes to `<cwd>/briefs/<slug>-org-brief.yaml`. Slug is the
kebab-case form of TARGET (≤ 16 chars).

When you reach Phase D's STOP gate, print to the operator:

- 3 highest-confidence claims
- 5 most material uncertainties
- 3 random `confidence: high` cross-check results

Wait for the operator's `ok, proceed` before flipping
`meta.status: ready`.

### Step 3 — STOP if MODE is research-only

The brief is the deliverable. Hand off:

```
✅ Brief ready: <cwd>/briefs/<slug>-org-brief.yaml
   meta.status: ready

   To continue into compose-org, re-run with MODE=full and the
   same TARGET — research-company will detect the existing brief
   and skip straight to compose-org.
```

### Step 4 — Run compose-org (MODE=full only)

Walk Phase 0 → Phase J of `skills/compose-org/SKILL.md`. Use:

- **brief**: `<cwd>/briefs/<slug>-org-brief.yaml`
- **substrate URL**: `https://github.com/arturcrmbot/zava-control-plane`
  (Phase A clones this into `<cwd>/zava-control-plane-<slug>/`)
- **fork target**: `<cwd>/zava-control-plane-<slug>/`

After every phase print: one-line summary, last commit SHA,
files-touched count.

### Step 5 — Hand off

When Phase J completes, print:

```
✅ Fork ready: <cwd>/zava-control-plane-<slug>/

   cd zava-control-plane-<slug>
   make install
   make up

   Visit http://localhost:5273
```

## Safety gates — STOP AND ASK before each

The agent must pause and wait for explicit operator approval
before each of these destructive steps:

1. **Before Phase A clone** — the substrate is ~50MB; confirm the
   fork target path with the operator first.
2. **Before Phase B rebrand find-and-replace** — show the operator
   the find/replace mapping table (derived from
   `brief.identity.short_name` and `brief.identity.slug`) and wait
   for `ok`.
3. **Before Phase D Kuzu schema swap** — show the entity-kind
   rename table from the primer; wait for `ok`.
4. **Before Phase H data-fabric re-seed** — this re-runs the
   pack-builder against the new generators. Confirm.
5. **Before Phase I `make test`** — confirm the operator wants
   tests run (~5 min wall clock).

## If a phase fails

STOP. Do not improvise a fix. Report:

- Which phase + sub-step.
- What command/diff failed (paste the actual error).
- Which line of the fetched SKILL.md / primer / brief is unclear
  or wrong.

The operator will fix the catalog file (and push to GitHub) and
re-run. Re-runs are idempotent because compose-org Phase 0
detects the existing fork and refuses to overwrite without
`--allow-overwrite`.

## Constraints

- **NO `git push`, NO `gh repo create`** for the produced fork.
  Local-only by default. The operator decides if/when to push.
- **NEVER edit a fetched skill, primer, or brief silently
  mid-run.** If you find something wrong, stop and tell the
  operator. The catalog is the source of truth; fixes go upstream
  via PR.
- **NEVER touch anything outside the operator's cwd** (other
  than your own clone of the substrate, which lands inside cwd).
- Stay agnostic in commit messages — no customer-name leakage
  beyond what the rebrand naturally introduces (which only
  appears inside the fork, never in this catalog).
- If the SKILL.md says X, do X verbatim. If X is unclear, ask
  the operator — don't guess.

## What the operator's prompt looks like

The operator typically pastes a prompt like this when starting:

```
Read https://raw.githubusercontent.com/arturcrmbot/zava-design-skills/main/RUNBOOK.md
and follow it end-to-end.

TARGET = <target>
MODE = full
```

Or for just the research half:

```
Read https://raw.githubusercontent.com/arturcrmbot/zava-design-skills/main/RUNBOOK.md
and follow it end-to-end.

TARGET = <target>
MODE = research-only
```

The agent (you) then fetches this file, reads it, and walks the
procedure above.
