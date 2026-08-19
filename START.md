# Ok let’s start

When a human says **“ok let’s start”** (or equivalent), follow this **before** application code.

Do not invent a second product. Grafana is not the UI. **Only Trevor merges** to `main`.

## Load order

1. This file
2. `PLAN.md` — product, judge path, HTTP + auth, **the named section for this human**
3. `handoffs/README.md` — per-PR handoff + collision. Then `gh pr list --state open` before any write.
4. `contracts/http.draft.md` until `contracts/http.md` exists
5. `skills/INDEX.md`

Then branch on who they are.

## Who

If they did not say, ask: **Trevor / Alexis / Michael**.

| Human | Constitution | First move on “let’s start” |
|---|---|---|
| Trevor | `skills/trevor-recorder/` (already filled) | That `SKILL.md` + **one** `agents/*.md`. Do not fill Alexis/Michael folders. |
| Alexis or Michael | **They fill it.** Trevor does not. | `skills/FILL-CONSTITUTION.md` until their folder exists and has no `<placeholders>`. |

## Alexis / Michael

1. Read [`skills/FILL-CONSTITUTION.md`](skills/FILL-CONSTITUTION.md) and run it.
2. Copy [`skills/lane-constitution/`](skills/lane-constitution/) → `skills/<your-lane>/`.
3. Fill **your** `SKILL.md`, stack, missions, `progress.md` from **your** PLAN section + HTTP. Same format, your content.
4. Do **not** clone `skills/trevor-recorder/`.
5. After every session, append to **your** `progress.md` so the constitution accumulates what you actually did.

**Scratchpad** (this GitHub repo if `TVault-scratchbook-accessible`): constitution only. No `sdk/`, `vault/`, `web/`, `infra/`.

**Product repo:** still no lane code until hour 0 locks contracts. Michael: no `web/` until `PRODUCT.md` exists.

When the constitution is already filled, skip FILL; execute the next mission in **their** `workflow.md`, collision-check open PRs, write `handoffs/<name>-<id>-<slug>.md` on that PR, append `progress.md`.

## Trevor

Read `skills/trevor-recorder/SKILL.md` and `workflow.md`. One mission. Do not write `vault/` or `web/`. Do not author Alexis/Michael skills.

Scratchpad: no `sdk/` / `infra/` / CI here either.

## Paste

**Alexis**

```
I am Alexis. Ok let's start.
Read START.md and skills/FILL-CONSTITUTION.md.
Copy skills/lane-constitution/ into my lane folder and fill it from PLAN.md (my section + HTTP + auth).
Do not copy skills/trevor-recorder/. Do not implement vault/ in the scratchpad.
Before any later write: gh pr list --state open and handoffs/README.md. One handoff file per PR.
Update my progress.md before you stop. Do not merge to main — Trevor merges.
```

**Michael**

```
I am Michael. Ok let's start.
Read START.md and skills/FILL-CONSTITUTION.md.
Copy skills/lane-constitution/ into my lane folder and fill it from PLAN.md (my section + HTTP + Look).
Do not copy skills/trevor-recorder/. Do not implement web/ in the scratchpad.
Before any later write: gh pr list --state open and handoffs/README.md. One handoff file per PR.
Update my progress.md before you stop. Do not merge to main — Trevor merges.
```

**Trevor**

```
I am Trevor. Ok let's start.
Read START.md, PLAN.md, handoffs/README.md, then skills/trevor-recorder/SKILL.md and one agents/*.md.
gh pr list --state open. If claimed paths overlap yours, stop.
Commit handoffs/<id>-<slug>.md on the PR (copy handoffs/PR.example.md).
Do not fill Alexis or Michael constitutions. Do not edit vault/ or web/.
Do not merge to main — Trevor merges feature branches.
```

Parallel paste after a lane exists: `AGENTS.md`.
