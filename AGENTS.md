# AGENTS.md — Contributor & Sub-Agent Guide

> Rules for anyone (human or AI) editing this repo. Read this **before**
> opening a PR, asking a sub-agent to refactor a skill, or running a mass
> scrub. Violating an invariant here breaks downstream consumers in subtle
> ways that won't fail CI.

This file mirrors the contributor guide in our sister catalog
[`awesome-gbb`](https://github.com/aiappsgbb/awesome-gbb/blob/main/AGENTS.md),
adapted to this catalog's narrower scope.

---

## 1 · Repo layout

```
zava-design-skills/
├── README.md                 # Public catalog index
├── AGENTS.md                 # ← you are here
├── LICENSE                   # MIT
├── briefs/                   # Per-engagement org-briefs (GITIGNORED)
└── skills/
    └── <skill-name>/
        ├── SKILL.md          # Skill definition (frontmatter + instructions)
        ├── README.md         # Optional extended docs
        ├── references/       # Optional: canonical industry data, schemas
        └── templates/        # Optional: copy-paste templates
```

No build step. Each skill is a self-contained markdown contract.

---

## 2 · Core invariants

### 2.1 Skills are agnostic

A skill must read sensibly to a stranger who has never seen your
customer, PoC, or fork. **Customer-specific output goes in `briefs/`,
which is gitignored.**

- ❌ NO customer names anywhere in skill bodies, frontmatter,
  references, examples (real or pseudonymous)
- ❌ NO PoC names (`<customer>-poc`, `acme-demo-v3`)
- ❌ NO real GUIDs, subscription IDs, tenant IDs, ARM IDs
- ✅ Industry names (Telco, FSI, MFG, Retail, Airline, Automotive,
  Healthcare, Utilities) are fine
- ✅ Generic placeholders (`<example-cardholder>`, `<insurer>`,
  `Contoso Bank`) are fine
- ✅ Microsoft product / SKU / region names are fine

If a piece of knowledge feels too tied to one customer to express
agnostically, the right home is a private fork or your engagement-
specific repo — **not this catalog**.

### 2.2 Reference data files are canon — do NOT normalize

Files under `skills/<skill>/references/industry-primers/<vertical>.md`
hold **deliberate published shorthand** documenting industry-standard
codes, formats, and conventions (regulator IDs, telco prefixes,
banking BIC/IBAN structure, etc.). They cite source specifications.

If you see something that "looks wrong" (a single-letter regulator
abbreviation, an unpadded number, an unusual capitalisation),
**STOP**. The shorthand is correct per the cited spec; do not
normalize it. Open a discussion if you genuinely believe the source
is wrong.

> The sister catalog `awesome-gbb` has paid for this lesson:
> a scrub sub-agent once rewrote `prefixes 0, 7-9, …` to
> `prefixes 00, 07-09, …` thinking it was "fixing formatting".
> That damaged canonical IRS Pub. 1635 shorthand. Don't.

### 2.3 Description ≤ 1024 chars

The `description:` field in SKILL.md frontmatter is what the runtime
uses to decide when to surface the skill. Most loaders cap at
**1024 characters** (some at 512). Count chars when editing.

### 2.4 SKILL.md frontmatter shape is fixed

Every `SKILL.md` MUST start with this exact YAML frontmatter shape:

```yaml
---
name: <kebab-case-skill-name>
description: >
  <Folded multi-line summary including USE FOR / DO NOT USE FOR clauses,
  200–1024 chars total>
metadata:
  version: "<semver>"
---
```

- `name` is the directory name under `skills/` — never rename without
  bumping MAJOR and updating every cross-reference
- `description: >` (folded scalar) preserves trigger phrases for
  fuzzy-match loaders without forcing one giant line
- `metadata.version` is required (see § 4 for SemVer rules)
- No other top-level frontmatter keys are recognized

### 2.5 Per-engagement output stays in `briefs/`

Output from a skill run against a specific customer (e.g. an
`org-brief.yaml` produced by `research-company`) lives under
`briefs/<slug>-org-brief.yaml` and is **gitignored**.

If you need to share a brief with a colleague, push it to a private
fork or transfer it out-of-band. **Never commit briefs to this
public-leaning catalog.**

---

## 3 · Editing checklist (run before every commit)

- [ ] **YAML frontmatter parses**:
      `python3 -c "import yaml,pathlib; [yaml.safe_load(p.read_text().split('---')[1]) for p in pathlib.Path('skills').rglob('SKILL.md')]"`
- [ ] **Description ≤ 1024 chars** on every touched SKILL.md
- [ ] **No customer / PoC names** introduced (`git diff | grep -iE 'colt|ryanair|bmw|telefonica|<other>'`)
- [ ] **No real GUIDs** introduced (`git diff | grep -E 'subscriptions/[0-9a-f]{8}-'`)
- [ ] **`metadata.version` present** and bumped per § 4 if user-facing
- [ ] **Cross-skill links resolve** — if you renamed a section header,
      grep the rest of the repo for stale `#section-name` anchors

---

## 4 · Versioning (`metadata.version`)

SemVer 2.0.0.

| Bump | When |
|---|---|
| **MAJOR** (1.0.0 → 2.0.0) | Renamed skill, removed a documented section, broke a downstream skill's contract |
| **MINOR** (1.0.0 → 1.1.0) | New documented section, new template/reference file, new `USE FOR` trigger |
| **PATCH** (1.0.0 → 1.0.1) | Typo, clarified wording, expanded example, fixed broken link |

Never set `0.x.y` — every skill in this repo is production-ready or
it doesn't ship.

---

## 5 · Mass-edit safety rails

Mass edits (scrubs, version bumps, metadata inserts) are the
highest-risk operation. Borrow the playbook from
[`awesome-gbb` § 4](https://github.com/aiappsgbb/awesome-gbb/blob/main/AGENTS.md#4--mass-edit--scrub-playbook-sub-agent-safety-rails):

1. State the mandate in one sentence.
2. Enumerate the explicit allow-list of file types the agent may touch.
3. Forbid normalization. Reference data is canonical.
4. Require a per-file change summary in the agent's output.
5. Inspect every modified file with `git diff -a` (forced text — `--stat` hides damage in UTF-8-heavy files).
6. Walk diffs line by line.

For edits to fewer than 5 files, do it yourself.

---

## 6 · See also

- [README.md](README.md) — catalog index
- [`awesome-gbb/AGENTS.md`](https://github.com/aiappsgbb/awesome-gbb/blob/main/AGENTS.md) — sister catalog, source of these conventions
- Each `skills/<skill>/SKILL.md` — the canonical contract for that skill
