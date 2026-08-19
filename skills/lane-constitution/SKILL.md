---
name: <lane>
description: >-
  Owns <one-sentence lane>. Use when working in <write-paths>.
compatibility: Cursor, Claude Code, Kiro, any Agent Skills loader
metadata:
  owner: <your-name>
  product: TraceVault
  lane: <lane>
---

# <your-name> lane — <lane title>

You are a coding agent on **<your-name>’s lane**.

If this file still contains `<placeholders>`, stop implementing. Run repo `START.md` and `skills/FILL-CONSTITUTION.md` first — fill this constitution from `PLAN.md` yourself. Do not copy `skills/trevor-recorder/`.

Read repo-root `PLAN.md` and this file before any edit. Then open **exactly one** mission file under `agents/` that matches your assigned id. Execute that mission. Do not wander into another mission.

If `PLAN.md` is missing, stop.

If the remote is `Sighopss/TVault-scratchbook-accessible`, you are in the **scratchpad**. Do not implement application code here. Product code goes in the separate repo named in `PLAN.md`.

## Product (do not invent another)

One product. Your job is the slice in PLAN.md under your name. Do not invent a second product. Do not take another human’s slice.

## Hard fences (non-negotiable)

**Write:** `<write-paths>`

**Read-only unless PLAN says you share it:** `contracts/`

**Never write:** paths PLAN.md assigns to someone else.

**Never:** commit secrets, force-push `main`, merge to `main` (Trevor merges), commit unless the human asked, start long-running servers.

If you need another lane changed: write `handoffs/<your-name>-<id>-<slug>.md` from [handoffs.md](handoffs.md) / repo `handoffs/PR.example.md`, put it on the PR, and stop.

## Parallel (one computer, many agents)

Claim a lease in `.agent-leases.json` (gitignored) before writing. One id, one path set. Details: [parallel.md](parallel.md).

| Your id | Mission file | Paths |
|---|---|---|
| `<your-name>-<id>` | [agents/<id>.md](agents/<id>.md) | `<paths>` |

Replace the table with **your** missions. Do not copy another lane’s table.

Human `<your-name>` may hold two adjacent path sets. Subagents must not.

Worktree: `.worktrees/<id>/` on branch `<your-name>/<id>/<slug>`. Open a PR. Do not merge — Trevor merges. Do not nest worktrees. Do not share a dirty tree.

## Stack (locked)

Fill [stack.md](stack.md) with **your** runtime and libs. Security bar: [enterprise.md](enterprise.md). Team I/O: [handoffs.md](handoffs.md). Steps: [workflow.md](workflow.md).

## After the mission

Release the lease. The per-PR handoff file is the handoff (claimed paths, shipped, blocked). Do not start the next mission unless the human named it.
