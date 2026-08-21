# Fill your constitution (Alexis / Michael)

Trevor does **not** write your lane folder. This file is the foothold for **your** LLM.

When the human says **“ok let’s start”** and you are Alexis or Michael, your **first job** is to copy the empty format and fill it from `PLAN.md`. Then, as you work, you keep writing into that constitution (missions, stack, progress). You do not wait for Trevor to invent your skills.

Do **not** copy `skills/trevor-recorder/` (bodies, agent ids, env lists, file trees, tests). Same **filenames** as `skills/lane-constitution/`. Different **content**. You may glance at Trevor’s folder only to see what “filled” looks like (every placeholder gone, real `agents/*.md`). Do not paste his text.

Grafana is not the UI. Do not invent a second product. Do not take another human’s git paths.

## Already filled?

If `skills/<your-lane>/SKILL.md` exists and has **no** `<placeholders>`, skip copy. Read that `SKILL.md` + `workflow.md` and continue from `progress.md`. Still append to `progress.md` every session.

If the folder is missing, or `SKILL.md` still has `<angle-brackets>`, this file is your procedure.

## Mode

**Scratchpad** (`Sighopss/TVault-scratchbook-accessible`): filling `skills/<your-lane>/` **is** the work. Do not create `vault/` or `web/` here.

**Product repo:** fill constitution first if it is not copied yet, then application code only after hour 0 (`contracts/http.md` + `contracts/span.schema.json` + full-flight fixtures). Michael: no `web/` until `PRODUCT.md` exists.

## Procedure (do in order)

### 1. Identity

Human must be Alexis or Michael. If unknown, ask. Trevor uses `skills/trevor-recorder/` — do not run this fill for him.

Pick a folder name. Keep it stable. Register it in `skills/INDEX.md` when you create it.

### 2. Copy the format

Copy **every file** from `skills/lane-constitution/` to `skills/<your-lane>/`. Keep names. Delete `agents/MISSION.example.md` only after you have written at least one real `agents/<id>.md` from it.

### 3. Sources of truth (read, then extract — do not invent)

| Read | You take |
|---|---|
| `PLAN.md` Done-bar, Judge path, Winners steal | constraints, kill-order, what judges click |
| `PLAN.md` **your named section** (Alexis or Michael) | write paths, numbered work, tests you owe |
| `PLAN.md` HTTP + auth | routes, error JSON, two Lambdas vs Explorer env — **only the rows you implement** |
| `PLAN.md` **Security + governance** | your rows only (Alexis: redaction/403/audit; Michael: no PII on screen) |
| `PLAN.md` **Handbook (2026)** | P-01–P-15, threat model, system card, demo notes — extract **your** tests/disclosures |
| `PLAN.md` Stack / Tree / Git / Prep / 48h table **your column** | runtime, CODEOWNERS, prep, hour windows |
| `PLAN.md` Look + `assets/tracevault-explorer-sample.png` | Michael only |
| `contracts/http.draft.md` | same HTTP until hour 0 copies it to `http.md` |
| `skills/lane-constitution/*` | filenames and parallel **rules** |

Shared HTTP is not “copy Trevor’s infra mission.” Alexis implements the vault rows. Michael consumes list/get/audit JSON and `NEXT_PUBLIC_*`. Trevor owns CORS, mock `/health`, zipping Lambdas, Cognito pool.

### 4. Fill files (you write the words)

Work through `SKILL.md`, then `ownership.md`, `parallel.md`, `stack.md`, `enterprise.md`, `handoffs.md`, `workflow.md`, `progress.md`. Replace every `<placeholder>`.

**SKILL.md:** product sentence for **your** slice; write/never-write paths from the three-person table; agent-id table **you** invent for **your** paths only.

**stack.md:** **your** language and libs from PLAN Stack. Env **names** you will need. No secret values. No other lane’s pin dump.

**enterprise.md:** **your** fail-closed path (Alexis: redaction + 403. Michael: no raw PII on screen, contracted 403 UI).

**handoffs.md:** fill the direction table for **your** I/O only. Per-PR files live in repo `handoffs/` — protocol in `handoffs/README.md`, not chat.

**workflow.md:** Start protocol = this file until constitution is filled, then one mission at a time. Done-list = acceptance from **your** PLAN bullets, not Trevor’s.

**progress.md:** living log. Seed one “copied format, filling from PLAN” entry. After **every** later session, append: did / files / learned / next / blocked. Never delete old entries. Durable facts also go into `stack.md` / `handoffs.md`.

**parallel.md:** keep the rules. Fill ids, paths, **your** mission order (dependencies you infer from PLAN). Paste block with **your** ids.

### 5. Missions (you split the work)

Do not use Trevor’s ids (`trevor-sdk`, …).

Method:

1. List PLAN implementation bullets under your name (skip the “copy constitution” bullet — that is this file).
2. Split so two agents never share a write path. One id, one `agents/<id>.md`, one path set.
3. For each mission, copy `agents/MISSION.example.md` → `agents/<id>.md` and fill Goal / Files / Behavior / Tests / Never from PLAN + HTTP. **You** name files and tests. The example file is the shape, not the content.
4. Put the id table in `SKILL.md` and the order in `workflow.md` / `parallel.md`.

If a PLAN bullet is too big for one context, split it. If two bullets share one file, they are one mission or you sequence them.

### 6. Wire the repo so the next session loads you

- `skills/INDEX.md` — your folder, not “not written yet”
- Repo `AGENTS.md` — paste block for **your** ids
- Mirror `SKILL.md` to `.cursor/skills/<your-lane>/SKILL.md`, `.claude/skills/<your-lane>/SKILL.md`, `.kiro/skills/<your-lane>/SKILL.md`

Do not edit `skills/trevor-recorder/`.

### 7. Stop conditions for the first “let’s start”

Constitution is fillable-complete when:

- No `<placeholders>` in your `SKILL.md`
- ≥1 real mission file
- `progress.md` has a first entry
- INDEX + AGENTS + tool-loader mirror done

Then stop and tell the human: constitution is yours; next “let’s start” runs **your** `workflow.md` (scratchpad: still no `vault/`/`web/` here; product repo + hour 0: next mission).

Do not implement application code in the same first pass unless the human said the product repo is ready **and** hour 0 is locked **and** the constitution is already filled.

## After the constitution exists

Every session:

1. `START.md` → `PLAN.md` → `handoffs/README.md` → `gh pr list --state open` (stop if claimed paths overlap) → your `SKILL.md` → one mission (or this fill file if still incomplete).
2. Do the work.
3. **Write back** into the constitution: tick workflow, append `progress.md`, add learned env/paths to `stack.md`.
4. Commit `handoffs/<your-name>-<id>-<slug>.md` on the PR (copy `handoffs/PR.example.md`). Paste it in the PR body.

That is the loop. Chat is not the system of record.

## Never

- Fill Alexis’s folder if you are Michael’s agent, or the reverse.
- Copy Trevor missions and rename them.
- Invent routes not in HTTP + auth.
- Merge to `main` (Trevor merges). Commit only if the human asked.
- Start long-running servers. Put secrets in git.
- Skip `gh pr list` / skip the per-PR `handoffs/*.md` file.
