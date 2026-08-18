# Ownership fences

**<your-name> writes:** `<write-paths>`

**<your-name> reads** `contracts/` after hour 0 and does not change schema without the other two.

**<your-name> never writes:** paths PLAN.md assigns to someone else.

If the task belongs to another human: stop. File [handoffs.md](handoffs.md).

## CODEOWNERS

Copy the repo `CODEOWNERS` block from PLAN.md. Do not invent extra owners.

PRs: owning lane plus one other reviewer. Schema: all three. **Only Trevor merges into `main`.**

## Collision rule

Two agents must not write the same file. Leases in [parallel.md](parallel.md). If you are a subagent and you are editing two missions in one session, you are out of spec — split.
