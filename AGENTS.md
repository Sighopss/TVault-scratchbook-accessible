# AGENTS

This repository is the TraceVault **scratchpad** (plan, brand, skills format, **Trevor** skills). It is not the product repo. Do not implement `sdk/`, `vault/`, or `web/` here.

You are an LLM coding agent. Do not invent a second product. Grafana is not the UI. Do not fill or edit another human’s lane folder.

## Start here every session

1. `START.md` if the human said let’s start (or first clone)
2. `README.md`
3. `PLAN.md`
4. `contracts/http.draft.md`
5. `skills/INDEX.md`
6. **Trevor:** `skills/trevor-recorder/SKILL.md` then **one** `agents/{sdk,demo,scripts,infra,ci}.md`
7. **Alexis / Michael:** `skills/FILL-CONSTITUTION.md` until their constitution exists and has no placeholders; then their `SKILL.md` + one mission

Trevor does not write Alexis or Michael skills.

## Paste — Alexis / Michael first session

See `START.md`. After they have a folder, they add **their** id pastes below (same shape as Trevor’s).

## Paste for Trevor parallel agents

```
You are trevor-sdk.
Read PLAN.md and skills/trevor-recorder/SKILL.md.
Execute skills/trevor-recorder/agents/sdk.md only.
Do not commit unless I ask. Do not merge to main — Trevor merges feature branches. Do not edit vault/ or web/.
Do not fill Alexis or Michael constitutions.
```

Change `sdk` to `demo`, `scripts`, `infra`, or `ci` for the other four agents. Run them in different worktrees. See `skills/trevor-recorder/parallel.md`.

## Tool loaders

Cursor: `.cursor/skills/<lane>/SKILL.md`  
Claude Code: `.claude/skills/<lane>/SKILL.md`  
Kiro: `.kiro/skills/<lane>/SKILL.md` and `.kiro/steering/` if present

Trevor’s copies are already in-tree. Alexis/Michael mirror theirs when they fill.
