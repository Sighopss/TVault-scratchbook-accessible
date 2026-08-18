# Parallel agents

Several LLMs on one machine collide unless you follow this. This is the shared protocol. Fill ids and paths for **your** lane only.

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

## Prompt to paste into a parallel agent

```
You are <your-name>-<id>.
Read PLAN.md and skills/<your-lane>/SKILL.md.
Execute skills/<your-lane>/agents/<id>.md only.
Do not commit unless I ask. Do not merge to main — Trevor merges.
Do not edit paths PLAN.md assigns to someone else.
```

Replace `<id>` per mission.
