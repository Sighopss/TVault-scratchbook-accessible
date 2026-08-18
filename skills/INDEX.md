# Skills index

Hackathon start: clone this repo, then load **one lane folder**, not the whole tree.

| Lane | Owner | Folder | Trigger paths |
|---|---|---|---|
| Recorder + AWS | Trevor | [trevor-recorder/](trevor-recorder/) | `sdk/`, `demo-app/`, `infra/`, `scripts/`, `.github/`, `Makefile` |
| Vault | Alexis | none yet | `vault/` — use `PLAN.md` until a skill exists |
| Explorer | Michael | none yet | `web/` — use `PLAN.md` + Impeccable until a skill exists |

## How an LLM should load Trevor

1. Read `PLAN.md` (repo root).
2. Read `skills/trevor-recorder/SKILL.md`.
3. Read a sibling **only if** the task matches:

| Task | Extra file |
|---|---|
| Path fences / CODEOWNERS | [ownership.md](trevor-recorder/ownership.md) |
| Many agents on one machine | [parallel.md](trevor-recorder/parallel.md) |
| Languages, SDK shape, Terraform | [stack.md](trevor-recorder/stack.md) |
| Security / AWS / CI bar | [enterprise.md](trevor-recorder/enterprise.md) |
| Alexis + Michael contracts | [handoffs.md](trevor-recorder/handoffs.md) |
| Step-by-step + done list | [workflow.md](trevor-recorder/workflow.md) |

Do not concatenate every sibling on every turn.

## Tool paths (same canonical folder)

| Tool | Loader |
|---|---|
| Any | `skills/trevor-recorder/SKILL.md` |
| Cursor | `.cursor/skills/trevor-recorder/SKILL.md` → points here |
| Claude Code | `.claude/skills/trevor-recorder/SKILL.md` → points here |
| Kiro | `.kiro/skills/trevor-recorder/SKILL.md` + `.kiro/steering/trevor-recorder.md` |
