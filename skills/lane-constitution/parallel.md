# Parallel agents

Several LLMs on one machine collide unless you follow this. These rules are the **shared format**. Do not rewrite them. Fill ids and paths for **your** lane only.

## Before first write

1. Choose **one** id from the table in `SKILL.md`.
2. Open **only** `agents/<that>.md`.
3. Create/update `.agent-leases.json` at **repo root** (gitignored). All lanes share this file.

```json
{
  "leases": [
    {
      "agent": "<your-name>-<id>",
      "paths": ["<write-paths>"],
      "host": "local",
      "started": "<iso8601>"
    }
  ]
}
```

If another lease overlaps your paths and `started` is less than four hours ago: **do not write**. Say you are blocked. Do not delete someone else’s lease.

Human `<your-name>` (interactive) may lease two adjacent path sets. Subagents: one id.

## Worktrees

```bash
git worktree add .worktrees/<your-name>-<id> -b <your-name>/<id>/<slug>
```

`.worktrees/` is gitignored. Branch names: `<your-name>/<id>/<slug>`. Open a PR to `main`. **Do not merge.** Trevor merges feature branches. Do not `git stash` another agent’s files. Do not nest worktrees. If `git rev-parse --git-common-dir` differs from `--git-dir`, you are already in a worktree — stay there.

## Order

You pick the order that matches **your** dependencies. Write it here. Do not copy another lane’s order.

Do not copy files between worktrees. Trevor merging the PR into `main` is the integrate step.

## Cross-machine (per PR)

Local leases do not protect Alexis’s laptop from Trevor’s. Before write: `gh pr list --state open`, read each PR’s **Claimed paths** (`handoffs/README.md`). Overlap → stop.

Every PR commits `handoffs/<your-name>-<id>-<slug>.md` (copy `handoffs/PR.example.md`) and pastes it in the PR body. Pickup: other LLM runs `gh pr view <N>`.

## Prompt to paste into a parallel agent

```
You are <your-name>-<id>.
Read PLAN.md, handoffs/README.md, and skills/<your-lane>/SKILL.md.
gh pr list --state open. If claimed paths overlap yours, stop.
Execute skills/<your-lane>/agents/<id>.md only.
Commit handoffs/<your-name>-<id>-<slug>.md on this PR (copy handoffs/PR.example.md).
Do not commit unless I ask. Do not merge to main — Trevor merges.
Do not edit paths PLAN.md assigns to someone else.
```

Replace `<id>` per mission.
