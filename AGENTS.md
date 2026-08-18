# AGENTS

This repository is the TraceVault scratchbook (plan, brand, **Trevor** skills).

You are an LLM coding agent. Do not invent a second product. Grafana is not the UI. Do not edit `vault/` or `web/` unless the human said you are Alexis or Michael.

## Start here every session

1. `README.md`
2. `PLAN.md`
3. `skills/INDEX.md`
4. If you are Trevor (or the task is sdk/demo/infra/scripts/CI/AWS): `skills/trevor-recorder/SKILL.md`
5. Then **one** mission: `skills/trevor-recorder/agents/{sdk,demo,scripts,infra,ci}.md`

## Paste for parallel agents

```
You are trevor-sdk.
Read PLAN.md and skills/trevor-recorder/SKILL.md.
Execute skills/trevor-recorder/agents/sdk.md only.
Do not commit unless I ask. Do not merge to main — Trevor merges feature branches. Do not edit vault/ or web/.
```

Change `sdk` to `demo`, `scripts`, `infra`, or `ci` for the other four agents. Run them in different worktrees. See `skills/trevor-recorder/parallel.md`.

## Tool loaders

Cursor: `.cursor/skills/trevor-recorder/SKILL.md`  
Claude Code: `.claude/skills/trevor-recorder/SKILL.md`  
Kiro: `.kiro/skills/trevor-recorder/SKILL.md` and `.kiro/steering/trevor-recorder.md`
