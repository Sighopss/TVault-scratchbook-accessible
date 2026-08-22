# AGENTS

This repository is the TraceVault **scratchpad** (plan, brand, skills format, **Trevor** skills). It is not the product repo. Do not implement `sdk/`, `vault/`, or `web/` here.

You are an LLM coding agent. Do not invent a second product. Grafana is not the UI. Do not fill or edit another human’s lane folder.

## Start here every session

1. `START.md` if the human said let’s start (or first clone)
2. `README.md`
3. `PLAN.md` — including **Handbook Cycle** and **Rubric 100** (every session)
4. `JUDGE.md` — never-kill + click path. Product-repo sessions: load from this scratchbook, do not copy the files over.
5. `handoffs/README.md` then `gh pr list --state open` (stop on claimed-path overlap)
6. `contracts/http.draft.md`
7. `skills/INDEX.md`
8. **Trevor:** `skills/trevor-recorder/SKILL.md` then **one** `agents/{sdk,demo,scripts,infra,ci}.md`
9. **Alexis / Michael:** `skills/FILL-CONSTITUTION.md` until their constitution exists and has no placeholders; then their `SKILL.md` + one mission

Every PR: commit `handoffs/<name>-<id>-<slug>.md` and paste it in the PR body. Trevor does not write Alexis or Michael skills.

## Paste — Alexis / Michael first session

See `START.md`. After they have a folder, they add **their** id pastes below (same shape as Trevor’s).

## Paste for Trevor parallel agents

```
You are trevor-sdk.
Read PLAN.md, JUDGE.md, handoffs/README.md, and skills/trevor-recorder/SKILL.md.
gh pr list --state open. If claimed paths overlap yours, stop.
Execute skills/trevor-recorder/agents/sdk.md only.
Commit handoffs/<name>-<id>-<slug>.md on this PR with Handbook evidence.
Do not commit unless I ask. Do not merge to main — Trevor merges feature branches. Do not edit vault/ or web/.
Do not fill Alexis or Michael constitutions.
```

Change `sdk` to `demo`, `scripts`, `infra`, or `ci` for the other four agents. Run them in different worktrees. See `skills/trevor-recorder/parallel.md`.

## Tool loaders

Cursor: `.cursor/skills/<lane>/SKILL.md`  
Claude Code: `.claude/skills/<lane>/SKILL.md`  
Kiro: `.kiro/skills/<lane>/SKILL.md` and `.kiro/steering/` if present

Trevor’s copies are already in-tree. Alexis/Michael mirror theirs when they fill.
