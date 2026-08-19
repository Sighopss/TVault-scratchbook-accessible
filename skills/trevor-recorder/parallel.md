# Parallel agents

This repo will be hammered by several LLMs on one machine. They collide unless you follow this.

## Before first write

1. Choose **one** id from the table in `SKILL.md`.
2. Open **only** `agents/<that>.md`.
3. Create/update `.agent-leases.json` at repo root (gitignored).

```json
{
  "leases": [
    {
      "agent": "trevor-sdk",
      "paths": ["sdk/"],
      "host": "local",
      "started": "2026-08-18T19:00:00Z"
    }
  ]
}
```

If another lease overlaps your paths and `started` is less than four hours ago: **do not write**. Say you are blocked. Do not delete someone else’s lease.

Human Trevor (interactive) may lease `infra/` + `.github/` together. Subagents: one id.

## Worktrees

```bash
git worktree add .worktrees/trevor-sdk -b trevor/sdk/golden-span
```

`.worktrees/` is gitignored. Branch names: `trevor/<id>/<slug>`. Open a PR to `main`. **Do not merge.** Trevor merges feature branches. Do not `git stash` another agent’s files. Do not nest worktrees. If `git rev-parse --git-common-dir` differs from `--git-dir`, you are already in a worktree — stay there.

## Order

Hour 0 humans: real `contracts/span.schema.json`. Until then use `span.schema.draft.json`.

Parallel immediately: `trevor-sdk`, `trevor-infra`, `trevor-ci`.

After SDK is importable: `trevor-demo`. After demo CLI exists: `trevor-scripts`.

Do not copy files between worktrees. Trevor merging the PR into `main` is the integrate step.

## Cross-machine (per PR)

Local `.agent-leases.json` does not see Alexis’s or Michael’s laptops. Before write: `gh pr list --state open`, read **Claimed paths** in each PR (`handoffs/README.md`). Overlap → stop.

Every PR commits `handoffs/trevor-<id>-<slug>.md` (copy `handoffs/PR.example.md`) and pastes it in the PR body.

## Prompt to paste into a parallel agent

```
You are trevor-sdk. Load PLAN.md, handoffs/README.md, and skills/trevor-recorder/SKILL.md.
gh pr list --state open. If claimed paths overlap yours, stop.
Execute skills/trevor-recorder/agents/sdk.md only.
Commit handoffs/trevor-sdk-<slug>.md on this PR.
Do not edit vault/ or web/. Do not commit unless I ask.
```

Replace `sdk` with `demo`, `scripts`, `infra`, or `ci`.
