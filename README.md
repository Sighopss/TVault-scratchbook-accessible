# TVault scratchbook

**Scratchpad only.** This GitHub repo is not the product.

**Theme:** Unified AI Observability  
**Product:** TraceVault — AI Application Flight Recorder  
**Team:** Trevor, Alexis, Michael (3)

Application code goes in a separate repo Trevor creates — see [PLAN.md](PLAN.md). Do **not** copy PLAN / [JUDGE.md](JUDGE.md) / skills-as-docs into that repo. Scoring stays here: PLAN **Handbook (2026)** + **JUDGE.md**. Every product PR still names Handbook evidence. Design and code must not break the judge never-kill list.

## Humans

Open [PLAN.md](PLAN.md) for the three-person product plan.

**Alexis / Michael:** paste the block in [START.md](START.md). Your LLM fills **your** constitution from PLAN. Trevor does not write it.

## LLMs — load in this order

1. This file
2. [START.md](START.md) — “ok let’s start”
3. [handoffs/README.md](handoffs/README.md) — one handoff file per PR; `gh pr list` before write
4. [AGENTS.md](AGENTS.md) — paste blocks once a lane exists
5. [PLAN.md](PLAN.md) — product, fences, 48h SaaS bar
6. [JUDGE.md](JUDGE.md) — click path, four metrics, never-kill, P-15
7. [contracts/http.draft.md](contracts/http.draft.md) — HTTP + auth (hour 0 lock)
8. [skills/INDEX.md](skills/INDEX.md)

**Trevor:** [skills/trevor-recorder/SKILL.md](skills/trevor-recorder/SKILL.md) then **one** mission under [skills/trevor-recorder/agents/](skills/trevor-recorder/agents/).

**Alexis / Michael:** [skills/FILL-CONSTITUTION.md](skills/FILL-CONSTITUTION.md) until your folder is filled. Copy [skills/lane-constitution/](skills/lane-constitution/). Do not copy `trevor-recorder/` content.

| Agent id | Mission | Writes |
|---|---|---|
| `trevor-sdk` | [agents/sdk.md](skills/trevor-recorder/agents/sdk.md) | `sdk/` |
| `trevor-demo` | [agents/demo.md](skills/trevor-recorder/agents/demo.md) | `demo-app/` |
| `trevor-scripts` | [agents/scripts.md](skills/trevor-recorder/agents/scripts.md) | `scripts/` |
| `trevor-infra` | [agents/infra.md](skills/trevor-recorder/agents/infra.md) | `infra/` |
| `trevor-ci` | [agents/ci.md](skills/trevor-recorder/agents/ci.md) | `.github/`, `Makefile` |

Each Trevor mission names exact files, APIs, tests, and bans. Do not open all five in one context. Alexis/Michael missions live in **their** folders after they fill them.

Draft span schema (until hour 0): [span.schema.draft.json](skills/trevor-recorder/span.schema.draft.json)  
HTTP contract (until hour 0): [contracts/http.draft.md](contracts/http.draft.md)

## Brand

[assets/](assets/) — marks and [Explorer look](assets/tracevault-explorer-sample.png) (how the product should look). Not application code.
