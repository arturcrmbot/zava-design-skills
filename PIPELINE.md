# Pipeline — End-to-End Customer Fork

> The two-skill pipeline that takes a customer name and produces a
> running digital-clone demo of the substrate.

Modelled on the [`awesome-gbb` Threadlight](https://github.com/aiappsgbb/awesome-gbb/blob/main/THREADLIGHT.md)
chain, but narrower: **two skills, one shared brief, one shared
industry primer, one resulting fork.**

## The chain

```
┌─────────────────────────────────────────────────────────────────────┐
│                         copilot session                                   │
│                                                                     │
│   Step 1                       Step 2                               │
│  ┌──────────────────┐         ┌──────────────────┐                  │
│  │ research-company │ ──────► │   compose-org    │                  │
│  └──────────────────┘         └──────────────────┘                  │
│         │                              │                            │
│         ▼                              ▼                            │
│  briefs/<slug>-              ../<substrate>-<slug>/                 │
│   org-brief.yaml             (local git repo, no remote)            │
│  (gitignored)                                                       │
└─────────────────────────────────────────────────────────────────────┘
                                        │
                                        ▼
                              make up   (operator)
                                        │
                                        ▼
                              http://localhost:5273
```

## Cross-skill contract

The contract between `research-company` and `compose-org` is the
**org-brief schema** documented in
[`skills/research-company/references/brief-schema.md`](skills/research-company/references/brief-schema.md):

| Producer | Field | Consumer |
|---|---|---|
| research-company | `meta.primer` (e.g. `"telco"`) | compose-org Phase 0 — picks which primer to load |
| research-company | `meta.status: ready` | compose-org Phase 0 — refuses to proceed otherwise |
| research-company | `identity.slug` | compose-org Phase A — names the fork dir |
| research-company | `identity.short_name` | compose-org Phase B — rebrand target token |
| research-company | `subsidiaries[]` | compose-org Phase C.1 — `SUBSIDIARIES` tuple |
| research-company | `leadership[]` (named ELT) | compose-org Phase E.2 — persona folder frontmatter |
| research-company | `strategic_themes[]` | compose-org Phase C.4 — narrative-arc seeds |
| research-company | `stack_overrides[]` | compose-org Phase G — generated Node mocks |
| primer | function table | compose-org Phase E.1 — `functions.py` |
| primer | regulator table | compose-org Phase J — surfaced in fork |
| primer | entity-kind set | compose-org Phase D — Kuzu schema |
| primer | proposed-domain library | compose-org Phase F — `domains.py` extensions |
| primer | rituals + KPIs | compose-org Phase C.3 + UI defaults |

If you change the schema, update **all** consumers (primer, compose-org
phase steps) in the same PR.

## Customer-workshop runbook

End-to-end, from a colleague handed an account name to a booting
demo fork. Typical wall-clock: 60–90 minutes.

### 1. Open a Copilot CLI session inside this catalog

```bash
cd zava-design-skills
copilot
```

### 2. Profile the target

> Read `skills/research-company/SKILL.md` and run it against
> `<target>.com`.

The agent runs the four-phase procedure (10–45 min depending on how
much the target publicly discloses) and writes
`briefs/<slug>-org-brief.yaml` with `meta.status: needs_review`.

### 3. Spot-review the brief

Look at the `uncertainties[]` block. Either accept each row or fix
it in-place. Then flip `meta.status` to `ready` (manually or via
the agent).

Typical edits:

- Verify subsidiary registered names against Companies House (one
  PDF download).
- Confirm any `confidence: low` claim with a Colt-side contact (or
  accept the uncertainty).
- Add any private stack knowledge as `stack_overrides[]` rows.

### 4. Fork the substrate

> Read `skills/compose-org/SKILL.md` and run it against
> `briefs/<slug>-org-brief.yaml`.

The agent runs the ten-phase procedure (15–30 min):

- Phase 0 — pre-flight (refuses dirty trees / existing forks)
- Phase A — clone substrate → `../<substrate>-<slug>/`
- Phase B — literal rebrand (one atomic commit)
- Phase C — data-fabric repack (subsidiaries, clients/brands,
  rituals, narrative arcs)
- Phase D — Kuzu schema swap (entity-kind tables)
- Phase E — functions + personae regeneration
- Phase F — domain composition (25+ stub domains)
- Phase G — stack mocks scaffolded
- Phase H — re-seed data fabric (rebuilds Kuzu snapshot)
- Phase I — smoke test (`make test`)
- Phase J — hand off

### 5. Boot the fork

```bash
cd ../<substrate>-<slug>
make up
```

UI at `http://localhost:5273`. The simulator immediately starts
spawning workflows from the substrate's generic domains plus any
domains that were promoted from stub to live during compose-org
(default: all kept as stubs — graduate them with `compose-domain`
inside the new fork as you need).

### 6. Optionally — graduate a hero domain

Inside the new fork:

```bash
cd <substrate>-<slug>
copilot
> Run compose-domain on `<workflow_type>` (which is currently a stub).
```

That fleshes out orchestrator / phase graphs / agent skills /
personae for one domain.

### 7. Demo prep

- Verify the fleet view in the UI shows the new domains.
- Boot the blueprint microsite (`http://localhost:5275`) and check
  the constellation surfaces the customer's branding.
- Capture a 30-min walkthrough.

## Failure modes

| Symptom | Probable cause | Fix |
|---|---|---|
| Phase 0 refuses to start: "brief.meta.status is not ready" | You forgot step 3. | Spot-review the brief, flip status. |
| Phase 0 refuses to start: "fork target already exists" | Re-running compose-org on the same brief. | Either `rm -rf ../<substrate>-<slug>` and re-run, or invoke with `--allow-overwrite`. |
| Phase B leaves stray `Zava` strings in tests | Allowlist miss. | Add the failing file's extension to the allowlist or its path to the targeted-paths list; re-run Phase B. |
| Phase D Kuzu schema errors at `make up` | Forgot to update projections in entity_projections.py. | Phase D.3 — apply the same find-and-replace inside that file. |
| Phase I tests fail with persona-registry mismatch | Phase E.2 missed a row. | Cross-check `personas.py` against `api/server/personae/`. |
| Boot fails: "no module named X" | One of the Node mocks is missing a dep. | Inside the fork: `cd mocks/<id> && npm install`. |

## Iterating

When a generated fork looks wrong:

- **Per-phase issues** → fix the phase prose in `compose-org/SKILL.md`, re-run.
- **Vertical-wide issues** (the primer's domain list, regulator
  catalogue, entity kinds) → fix the primer, re-run compose-org.
- **Company-specific issues** → fix the brief, re-run compose-org.

The brief + primer are the source of truth; the fork is derived.

## See also

- [`README.md`](README.md) — catalog index
- [`AGENTS.md`](AGENTS.md) — contributor invariants
- [`skills/research-company/`](skills/research-company/) — Step 1
- [`skills/compose-org/`](skills/compose-org/) — Step 2
- [`skills/research-company/references/industry-primers/`](skills/research-company/references/industry-primers/) — vertical canon
